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

## MITRE ATT&CK

- T1552.002 Credentials in Registry

## Tools

Registry Explorer, regedit, RECmd, reg query. Open the .reg in a text editor to read the keys directly.

## Signature

Forensic Sample Blueprint | qayssarayra.com
