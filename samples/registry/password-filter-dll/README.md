# Password Filter Dll

## Scenario

A password filter DLL (pwdflt) is added to LSA Notification Packages, so it sees passwords in cleartext at change time.

Host: WS-FIN-07  
Evidence: `registry.reg` (exported registry keys)  
Approx hive last write: 2024-12-27 14:33

## Timeline

Notification Packages now lists an extra DLL name beside scecli.

## How to spot it

- An extra name in LSA Notification Packages
- DLL sees plaintext on password change
- Normal value is just scecli
- Persists and harvests credentials

## Questions

```
1. Key?               Lsa\Notification Packages
2. Added?              pwdflt
3. Sees?               Plaintext passwords
4. When?               Password change
5. Technique?          Password filter DLL
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x cafc84c2a325da3193491590970a37e611a4419857b9b858d07e7893fe569a6a.zip -phidden` (or your archive tool). You get `registry.reg`.
2. Read it as text: `cat registry.reg`, or import into a throwaway hive and open in Registry Explorer / RECmd.
3. Jump to the key that matters: `grep -niA2 -E 'Run|AppInit|Userinit|Debugger|ServiceDll|Security Packages|AlwaysInstallElevated|DefaultPassword|USBSTOR' registry.reg` (narrow to this one).
4. Confirm against the answer: An extra name in LSA Notification Packages

## MITRE ATT&CK

- T1556.002 Password Filter DLL

## Tools

Registry Explorer, regedit, RECmd, reg query. Open the .reg in a text editor to read the keys directly.

## Signature

Forensic Sample Blueprint | qayssarayra.com
