# Userinit Logon Script

## Scenario

A UserInitMprLogonScript value runs a batch file at logon, a quiet per user persistence spot.

Host: WS-FIN-07  
Evidence: `registry.reg` (exported registry keys)  
Approx hive last write: 2022-07-14 13:30

## Timeline

An Environment key value UserInitMprLogonScript pointing at an AppData batch file.

## How to spot it

- A UserInitMprLogonScript value under HKCU Environment
- Runs a script at every logon
- Uncommon value that stands out
- Per user, needs no admin

## Questions

```
1. Key?               HKCU Environment
2. Value?              UserInitMprLogonScript
3. Target?             AppData\s\logon.bat
4. Runs at?            Logon
5. Technique?          Logon script persistence
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 00953861877421901b2410fe69b875c3de0b083ccd6eea277fbe718b3c0595fc.zip -phidden` (or your archive tool). You get `registry.reg`.
2. Read it as text: `cat registry.reg`, or import into a throwaway hive and open in Registry Explorer / RECmd.
3. Jump to the key that matters: `grep -niA2 -E 'Run|AppInit|Userinit|Debugger|ServiceDll|Security Packages|AlwaysInstallElevated|DefaultPassword|USBSTOR' registry.reg` (narrow to this one).
4. Confirm against the answer: A UserInitMprLogonScript value under HKCU Environment

## MITRE ATT&CK

- T1037.001 Logon Script (Windows)

## Tools

Registry Explorer, regedit, RECmd, reg query. Open the .reg in a text editor to read the keys directly.

## Signature

Forensic Sample Blueprint | qayssarayra.com
