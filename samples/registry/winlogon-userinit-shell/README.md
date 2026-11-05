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

## MITRE ATT&CK

- T1547.004 Winlogon Helper DLL

## Tools

Registry Explorer, regedit, RECmd, reg query. Open the .reg in a text editor to read the keys directly.

## Signature

Forensic Sample Blueprint | qayssarayra.com
