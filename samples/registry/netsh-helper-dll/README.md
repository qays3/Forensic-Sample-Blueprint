# Netsh Helper Dll

## Scenario

A Netsh helper DLL is registered so the DLL loads every time netsh runs.

Host: WS-FIN-07  
Evidence: `registry.reg` (exported registry keys)  
Approx hive last write: 2019-12-08 16:01

## Timeline

A new value under the Netsh key naming a ProgramData DLL.

## How to spot it

- A Netsh helper value pointing at a non system DLL
- Loads whenever netsh is invoked
- Netsh runs often in scripts and by admins
- Few legit third party helpers exist

## Questions

```
1. Key?               SOFTWARE\Microsoft\Netsh
2. DLL?                ProgramData\nh\helper.dll
3. Loads when?          netsh runs
4. Why useful?          netsh runs often
5. Technique?          Netsh helper DLL
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 1040d50ca7b481606d0ddec77bb78c8aa41af47efcb48f3eda4ba7ff3dcfe2b4.zip -phidden` (or your archive tool). You get `registry.reg`.
2. Read it as text: `cat registry.reg`, or import into a throwaway hive and open in Registry Explorer / RECmd.
3. Jump to the key that matters: `grep -niA2 -E 'Run|AppInit|Userinit|Debugger|ServiceDll|Security Packages|AlwaysInstallElevated|DefaultPassword|USBSTOR' registry.reg` (narrow to this one).
4. Confirm against the answer: A Netsh helper value pointing at a non system DLL

## MITRE ATT&CK

- T1546.007 Netsh Helper DLL

## Tools

Registry Explorer, regedit, RECmd, reg query. Open the .reg in a text editor to read the keys directly.

## Signature

Forensic Sample Blueprint | qayssarayra.com
