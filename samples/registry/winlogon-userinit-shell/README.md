# Winlogon Userinit Shell

## Scenario

Winlogon Userinit and Shell carry an extra binary after the normal value, so the payload launches with the session.

Host: WS-FIN-07  
Evidence: `registry.reg` (exported registry keys)  
Approx hive last write: 2023-08-19 13:41

## Timeline

Userinit ends with an extra path after userinit.exe; Shell has an extra binary after explorer.exe.

## How to spot it

- A second path appended to Userinit after userinit.exe
- An extra binary appended to Shell after explorer.exe
- Both run at every interactive logon
- Values normally hold exactly one entry

## Questions

```
1. Keys?              Winlogon Userinit and Shell
2. Tell?               Extra path appended to each
3. Runs at?            Every logon
4. Payload?            ProgramData\win\*.exe
5. Technique?          Winlogon helper persistence
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x a0d80d03b9e37091f5abdec69028a7208db95a4d44f93692deb517056421d7c9.zip -phidden` (or your archive tool). You get `registry.reg`.
2. Read it as text: `cat registry.reg`, or import into a throwaway hive and open in Registry Explorer / RECmd.
3. Jump to the key that matters: `grep -niA2 -E 'Run|AppInit|Userinit|Debugger|ServiceDll|Security Packages|AlwaysInstallElevated|DefaultPassword|USBSTOR' registry.reg` (narrow to this one).
4. Confirm against the answer: A second path appended to Userinit after userinit.exe

## MITRE ATT&CK

- T1547.004 Winlogon Helper DLL

## Tools

Registry Explorer, regedit, RECmd, reg query. Open the .reg in a text editor to read the keys directly.

## Signature

Forensic Sample Blueprint | qayssarayra.com
