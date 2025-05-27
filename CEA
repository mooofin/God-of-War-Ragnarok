{ Game   : GoWR.exe
  Version:
  Date   : 2024-09-23
  Author : muffin

  This script does blah blah blah
}

[ENABLE]
//////////

//aobscanmodule(addresource,GoWR.exe,45 33 C0 48 8B CF 41 8D 50 06) // should be unique
//registersymbol(addresource)
// GoWR.exe+DFF4D0 Recipe
// GoWR.exe+E015F0 Equip

aobscanmodule(resources_GetValidResourceId,$process,48 89 5C 24 10 57 48 83 EC 20 48 8B FA C7 02 FF FF FF FF 48 8B D9)
registersymbol(resources_GetValidResourceId)

aobscanmodule(StdNameHash,$process,33 C0 4C 8B D1 48 89 01 48 85 D2 74 56 44 0F B6 0A 45 84 C9 74 4D 4C 8D 5A 01 66 0F 1F 44 00 00)
registersymbol(StdNameHash)

aobscanmodule(Wallet_ModifyResource,$process,48 83 EC 38 0F B6 44 24 60 88 44 24 28 4C 89 4C 24 20 41 B1 01)
registersymbol(Wallet_ModifyResource)

aobscanmodule(progression_skill_tree__GetSkillTree,$process,48 83 EC 38 48 85 C9 75 17)
registersymbol(progression_skill_tree__GetSkillTree)

aobscanmodule(resources_FindWallet,$process,44 8B 0D ?? ?? ?? ?? 33 D2 45 85 C9 7E 1A)
registersymbol(resources_FindWallet)

// sound::wwise::Server::UpdateRTPCs
aobscanmodule(aobGetPlayer,$process,48 8B 05 ?? ?? ?? ?? 48 85 C0 0F 84 62 03 00 00 48 8B 78 08)
registersymbol(aobGetPlayer)

label(goPlayer__sm_pPlayer)
registersymbol(goPlayer__sm_pPlayer)

aobGetPlayer+7+(DWORD)[aobGetPlayer+3]:
goPlayer__sm_pPlayer:

aobscanmodule(aobGetTop,GoWR.exe,48 8B 41 20 48 8B 51 10) // should be unique
alloc(newmem,$1000,aobGetTop)

label(code)
label(return)
label(bCall)
registersymbol(bCall)
label(iQty)
registersymbol(iQty)
label(fDamageMul)
registersymbol(fDamageMul)
label(iResourceID)
registersymbol(iResourceID)
label(WalletHash)
registersymbol(WalletHash)
label(ResourceHash)
registersymbol(ResourceHash)
newmem:

code:
 cmp byte ptr[bCall],0
  je Skip
  mov byte ptr[bCall],0
   push rcx
   push rdx
   push rbx
   sub rsp,80
   mov rcx,[WalletHash]
   call resources_FindWallet
   test rax,rax
   je _Null
   mov rbx,rax
   lea rdx,[rsp+20]
   mov rcx,[ResourceHash]
   call resources_GetValidResourceId
   test al,al
   je _Null
   mov rdx,[rsp+20]
   mov rcx,rbx
   //mov rdx,[iResourceID]
   mov r8,[iQty]
   mov r9,pTele
   mov [rsp+20],0
   call Wallet_ModifyResource
 _Null:
   add rsp,80
   pop rbx
   pop rdx
   pop rcx
  Skip:
  mov rax,[rcx+20]
  mov rdx,[rcx+10]
  jmp return
align 10 00
szTele:
db 'NO_TELEMETRY'
db 0
WalletHash:
dq 00004895C643225F // "HERO"
ResourceHash:
dq 9C7CF61C839B9A8A // Kratos XP "EconomyXP"
iResourceID:
dd 0 // Default: Kratos XP
fDamageMul:
dd (float)2
iQty:
dd #10
bCall:
db 0
pTele:
dq 0
dq szTele

aobGetTop:
  jmp newmem
  nop 3
return:
registersymbol(aobGetTop)

[DISABLE]

aobGetTop:
  db 48 8B 41 20 48 8B 51 10

unregistersymbol(*)
dealloc(newmem)

