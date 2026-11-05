# Autologon Defaultpassword

## Scenario

AutoLogon is configured with the password stored in cleartext in DefaultPassword, readable by anyone with the hive.

Host: WS-FIN-07  
Evidence: `registry.reg` (exported registry keys)  
Approx hive last write: 2022-01-11 12:26

## Timeline

Winlogon AutoAdminLogon enabled with a cleartext DefaultPassword value.

## How to spot it

- AutoAdminLogon set to 1
- A DefaultPassword value in cleartext
- Admin username stored alongside
- Credentials recoverable straight from the hive

## Questions

```
1. Key?               Winlogon
2. Values?             AutoAdminLogon, DefaultPassword
3. Password?           Cleartext in the registry
4. Account?            Administrator
5. Technique?          Credentials in registry
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 90f3b05aeff77a6c6ba581a4c0781b95c84103ac93fc553b2cba136e175f2225.zip -phidden` (or your archive tool). You get `registry.reg`.
2. Read it as text: `cat registry.reg`, or import into a throwaway hive and open in Registry Explorer / RECmd.
3. Jump to the key that matters: `grep -niA2 -E 'Run|AppInit|Userinit|Debugger|ServiceDll|Security Packages|AlwaysInstallElevated|DefaultPassword|USBSTOR' registry.reg` (narrow to this one).
4. Confirm against the answer: AutoAdminLogon set to 1

## MITRE ATT&CK

- T1552.002 Credentials in Registry

## Tools

Registry Explorer, regedit, RECmd, reg query. Open the .reg in a text editor to read the keys directly.

## Signature

Forensic Sample Blueprint | qayssarayra.com
