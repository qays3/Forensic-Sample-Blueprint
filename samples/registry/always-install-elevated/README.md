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

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 688fbc47918f25e575091aac954e66d61dc07f981c9dd61e064664ccf7f06bb1.zip -phidden` (or your archive tool). You get `registry.reg`.
2. Read it as text: `cat registry.reg`, or import into a throwaway hive and open in Registry Explorer / RECmd.
3. Jump to the key that matters: `grep -niA2 -E 'Run|AppInit|Userinit|Debugger|ServiceDll|Security Packages|AlwaysInstallElevated|DefaultPassword|USBSTOR' registry.reg` (narrow to this one).
4. Confirm against the answer: AlwaysInstallElevated = 1 in HKLM and HKCU

## MITRE ATT&CK

- T1548.002 Bypass UAC
- T1078 Valid Accounts

## Tools

Registry Explorer, regedit, RECmd, reg query. Open the .reg in a text editor to read the keys directly.

## Signature

Forensic Sample Blueprint | qayssarayra.com
