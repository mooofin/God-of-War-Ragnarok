CE operates entirely in userspace via `ReadProcessMemory`/`WriteProcessMemory`. Everything here assumes a user-mode target with no kernel anti-cheat.

**Memory scanning pipeline:**
1. Exact/unknown-value scan → differential narrowing → single address
2. *Find what writes* → breakpoint on address → identify responsible instruction
3. AOB the instruction's surrounding bytes → derive pattern for portability across sessions
4. Inject via code cave or NOP depending on goal

**Pointer chain resolution** (surviving ASLR + heap realloc):
```
[module_base + static_offset] → ptr → +0x28 → ptr → +0x4C = target
```
Use CE's Pointer Scanner with *compare across sessions* to validate chain stability.

---

## 2. Auto-Assembler Basics

CE's AA syntax - not C, not MASM. Know the difference.

```asm
[ENABLE]
aobscanmodule(LABEL, target.exe, 29 48 4C 89 45 FC)
// derives address from byte pattern — must match exactly, get bytes from Memory Viewer

alloc(cave, 128, LABEL)   // allocates within ±2GB of LABEL for rel32 jmp
label(ret)

cave:
  // replacement logic
  mov [rbp-4], eax
  jmp ret

LABEL:
  jmp cave
  nop 2             // pad if jmp shorter than overwritten region

ret:
[DISABLE]
LABEL:
  db 29 48 4C 89 45 FC   // restore original bytes
dealloc(cave)
[/DISABLE]
```

**Rules:**
- Always restore bytes in `[DISABLE]`
- Pad overwritten region to original instruction size
- AOB from the actual binary, never guess

---

## 3. Common Patterns

### NOP injection (e.g. health decrement)
Find write → *Find what writes* → identify `sub [rax+4C], ecx` → NOP or cave past it.

### Value freeze
Lua timer  runs in CE's context via WPM, not in-thread:
```lua
local addr = getAddress("target.exe") + 0x1A3F20
writeFloat(addr + 0x2A4, 999.0)
-- wrap in createTimer() if continuous
```
For frame-accurate writes, use an AA hook instead of Lua.

### VTable swap
vtable page is `PAGE_READONLY` - `VirtualProtect` before writing, restore after:
```cpp
uintptr_t* vt = *(uintptr_t**)obj;
VirtualProtect(vt + idx, 8, PAGE_READWRITE, &old);
vt[idx] = (uintptr_t)hook_fn;
VirtualProtect(vt + idx, 8, old, &tmp);
```
Find vtable index via ReClass.NET + cross-ref in Ghidra/IDA.

### Conditional branch patch
```asm
// je → jne: flip 0x74 to 0x75 (short jump)
// jz  → nop nop: 74 XX → 90 90
```

---

## 4. Anti-Debug (target-side)

| Check | Mechanism | Bypass |
|---|---|---|
| `IsDebuggerPresent` | PEB.BeingDebugged flag | Patch return to 0 or use ScyllaHide |
| `CheckRemoteDebuggerPresent` | `NtQueryInformationProcess(ProcessDebugPort)` | ScyllaHide / hook ntdll |
| Timing (`RDTSC`) | Delta between calls inflated under debugger | Hook / fake RDTSC |
| Heap flags | `NtGlobalFlag` in PEB | Zero out the flags |
| `INT3` self-check | SEH-based detection | Handle in debugger, pass exception |

CE's VEH debugger backend avoids some detections vs. the standard debug API. ScyllaHide covers most of the rest for x64dbg.

**Kernel AC (EAC, BattleEye):** These register callbacks at kernel level (`PsSetLoadImageNotifyRoutine`, `ObRegisterCallbacks`). Bypassing them requires a signed driver, BYOVD, or hypervisor  out of scope here.

---

## 5. Toolchain

| Tool | Role |
|---|---|
| CE 7.5+ | Scan, AA injection, Lua |
| x64dbg + ScyllaHide | Dynamic analysis, anti-debug bypass |
| ReClass.NET | Struct layout reconstruction |
| Ghidra / IDA | Static analysis, vtable indexing |
| System Informer | Handle/memory map inspection |

---

