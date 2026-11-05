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

## MITRE ATT&CK

- T1037.001 Logon Script (Windows)

## Tools

Registry Explorer, regedit, RECmd, reg query. Open the .reg in a text editor to read the keys directly.

## Signature

Forensic Sample Blueprint | qayssarayra.com
