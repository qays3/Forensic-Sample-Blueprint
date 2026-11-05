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

## MITRE ATT&CK

- T1556.002 Password Filter DLL

## Tools

Registry Explorer, regedit, RECmd, reg query. Open the .reg in a text editor to read the keys directly.

## Signature

Forensic Sample Blueprint | qayssarayra.com
