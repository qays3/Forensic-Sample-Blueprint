# Lsa Auth Package

## Scenario

A rogue Authentication Package is added to LSA so an attacker DLL loads into lsass at boot.

Host: WS-FIN-07  
Evidence: `registry.reg` (exported registry keys)  
Approx hive last write: 2019-03-26 12:32

## Timeline

Authentication Packages now includes an extra name beside msv1_0.

## How to spot it

- An extra name in LSA Authentication Packages
- DLL loaded into lsass at startup
- Normal value is just msv1_0
- Deep persistence in a trusted process

## Questions

```
1. Key?               Lsa\Authentication Packages
2. Added?              evilpk
3. Loads into?          lsass at boot
4. Normal value?        msv1_0
5. Technique?          LSA authentication package
```

## MITRE ATT&CK

- T1547.002 Authentication Package

## Tools

Registry Explorer, regedit, RECmd, reg query. Open the .reg in a text editor to read the keys directly.

## Signature

Forensic Sample Blueprint | qayssarayra.com
