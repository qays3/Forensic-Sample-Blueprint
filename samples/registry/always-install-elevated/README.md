# Always Install Elevated

## Scenario

Both AlwaysInstallElevated keys are set, letting any user install an MSI as SYSTEM, a direct privilege escalation.

Host: WS-FIN-07  
Evidence: `registry.reg` (exported registry keys)  
Approx hive last write: 2020-07-05 14:27

## Timeline

AlwaysInstallElevated set to 1 under both HKLM and HKCU.

## How to spot it

- AlwaysInstallElevated = 1 in HKLM and HKCU
- Both must be set for the escalation
- Any MSI then runs as SYSTEM
- A misconfiguration attackers hunt for

## Questions

```
1. Values?            AlwaysInstallElevated (HKLM + HKCU)
2. Both set?           Yes (1)
3. Effect?             MSI as SYSTEM
4. Who can abuse?       Any user
5. Technique?          AlwaysInstallElevated
```

## MITRE ATT&CK

- T1548.002 Bypass UAC
- T1078 Valid Accounts

## Tools

Registry Explorer, regedit, RECmd, reg query. Open the .reg in a text editor to read the keys directly.

## Signature

Forensic Sample Blueprint | qayssarayra.com
