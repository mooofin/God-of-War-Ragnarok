# Cheat Engine Cheat Table Usage Guide

## Introduction
Cheat Engine (CE) is an advanced memory scanning, disassembly, and debugging tool that facilitates runtime memory modification through manual manipulation or automated Cheat Tables (`.ct` files). These structured scripts leverage pointer resolution, assembly injection, and real-time value overwriting to achieve deterministic or heuristic-based memory alterations. This guide provides a highly technical overview of the optimal workflow for harnessing Cheat Tables to manipulate volatile memory states efficiently.

## Prerequisites
- **Cheat Engine** (latest stable build, or a custom-compiled version for maximum control)
- **Target Process** (i.e., a running instance of the game binary within memory)
- **Cheat Table (`.ct` file)** tailored for the game's version, architecture (x86/x64), and memory layout
- **Knowledge of Assembly (optional but recommended for ASM script modifications)**

## Workflow: Utilizing a Cheat Table in Cheat Engine

### 1. Acquire the Cheat Table
- Download or create a `.ct` file that corresponds with the game’s memory structure.
- If utilizing a manually constructed Cheat Table, ensure the pointer chains are resolved properly.
- (Optional) Store `.ct` files within CE’s working directory for rapid access.

### 2. Launch Cheat Engine
- Execute `CheatEngine.exe` with elevated privileges (Administrator mode recommended for kernel-level access).

### 3. Initialize Target Process
- Launch the game and allow all relevant memory regions to be allocated.
- For deterministic manipulation, avoid state-changing events before scanning.

### 4. Attach Cheat Engine to the Target Process
- Use `ALT+TAB` to bring Cheat Engine into focus.
- Click the **Process List** icon (depicted as a PC icon in CE’s UI).
- Identify and select the active process for the target executable (e.g., `GameX.exe`).
- Click **Open** to hook Cheat Engine’s debugger into the memory space.

### 5. Load the Cheat Table
#### If the Cheat Table is in Cheat Engine’s Working Directory:
- CE automatically detects an associated `.ct` file and prompts for auto-load.
- Click **YES** to proceed.

#### If the Cheat Table Resides in an External Directory:
- Press `CTRL + O` or navigate to `File` > `Load`.
- Locate and select the `.ct` file.
- Click **Open** to import the script.

### 6. Activate Scripts and Modify Memory Values
- The Cheat Table will display a structured breakdown of scriptable modifications and memory regions.
- **To enable a script:** Check the activation box next to the relevant cheat (e.g., `God Mode`).
- **To modify direct memory values:**
  - Locate the corresponding address (e.g., `Money: 5000`).
  - Double-click the value field and manually override the stored value.
  - If necessary, apply a freeze (`Lock`) to prevent value restoration via game logic.

## Use Cases & Advanced Examples

### Example 1: Enabling God Mode via ASM Injection
1. Attach Cheat Engine to the process.
2. Load the Cheat Table.
3. Locate the `God Mode - ASM Injection` script.
4. Enable the script, triggering an `MOV EAX, 999` override within the health register.
5. Resume gameplay and confirm the applied invulnerability.

### Example 2: Locating a Dynamic Address and Hooking It
1. Perform an **Initial Scan** using `Exact Value` for a known in-game metric (e.g., `Health: 100`).
2. Take damage to modify the memory state.
3. Perform a **Next Scan** with the updated value.
4. Iterate scanning until a singular memory address remains.
5. Right-click the address and select **Add to Address List**.
6. Modify the value manually or apply a pointer scan to locate the base pointer.
7. (Advanced) Inject an inline `NOP` operation to suppress decrementing behavior.

### Example 3: Unlimited Ammo
1. Identify the memory address that stores the ammo count.
2. Fire a shot and scan for the updated value.
3. Continue refining scans until you isolate the correct memory address.
4. Lock the value or inject an ASM script to set it permanently high.

### Example 4: Speed Manipulation
1. Find the memory variable associated with movement speed.
2. Modify it to increase or decrease player velocity dynamically.
3. Lock the value to maintain persistent effects.

### Example 5: Jump Height Adjustment
1. Locate the jump height memory address.
2. Modify the value to allow for higher jumps.
3. Inject a script that applies a multiplier for dynamic height control.

### Example 6: No Recoil/Spread
1. Identify the recoil control memory region.
2. Apply a `NOP` operation to disable spread and recoil mechanics.

### Example 7: Enemy AI Freezing
1. Locate the memory value controlling enemy movement.
2. Modify it to prevent AI from executing movement logic.

### Example 8: Unlimited Skill Points
1. Scan for the current skill points value.
2. Modify and lock it to prevent depletion.

### Example 9: One-Hit Kills
1. Locate the enemy health memory region.
2. Modify the value to 1 or inject an ASM script to apply a massive damage multiplier.

### Example 10: Infinite Stamina
1. Scan for the current stamina value.
2. Modify and freeze it to prevent depletion.

### Example 11: Unlocking Paid DLC Content
1. Identify flags corresponding to DLC activation.
2. Modify them to enable locked features.

### Example 12: Map Clipping / No Collision
1. Locate and modify the player collision flag.
2. Set it to 0 or `NOP` collision checks.

### Example 13: Time Manipulation
1. Locate the in-game time variable.
2. Modify it to freeze or accelerate time flow.

### Example 14: XP Multiplier
1. Identify XP gain addresses.
2. Modify the gain amount before it is applied.

### Example 15: Unlocking Developer Console
1. Locate debug flag in memory.
2. Modify the value to enable hidden debugging tools.

## Debugging & Troubleshooting
- **Process Not Detectable:** Ensure the game is running with `Run as Administrator`. If the game employs anti-debugging mechanisms, use CE’s **Stealth Mode**.
- **Cheat Table Not Functioning:** Validate the `.ct` file against the game version; address offsets often shift post-patch.
- **Values Revert Instantly:** Some memory regions utilize dynamic allocation; use a pointer scan to locate the base address.
- **Game Crashes on Modification:** Certain memory spaces employ integrity checks; consider debugging and identifying checksum routines.

## Security & Ethical Considerations
- **Use Ethically:** Modifying single-player memory is generally acceptable, but altering networked values in multiplayer environments may trigger bans.
- **Anti-Cheat Mechanisms:** Games with kernel-mode anti-cheats (e.g., Vanguard, EAC, BattleEye) may flag CE as a threat.
- **Memory Integrity Checks:** Some modern titles use signature-based validation to detect modified instructions; apply script obfuscation or bypass mechanisms where applicable.
- **Avoid Permanent Data Corruption:** Always maintain backups of save files prior to executing memory modifications.

### Further Reading & Resources
- [Cheat Engine Official Documentation](https://www.cheatengine.org/)
- [Fearless Revolution - Cheat Table Repository](https://fearlessrevolution.com/)
- [x64dbg – Advanced Debugging Alternative](https://x64dbg.com/)
- [WinDbg – Windows Debugging Toolkit](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/)
- [ReClass.NET – Memory Structure Analysis](https://github.com/ReClassNET/ReClass.NET)

