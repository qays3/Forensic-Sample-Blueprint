# Com Hijack Inprocserver

## Scenario

A per user CLSID InprocServer32 override points a COM object at an attacker DLL, so it loads when that component is used.

Host: WS-FIN-07  
Evidence: `registry.reg` (exported registry keys)  
Approx hive last write: 2019-09-15 14:37

## Timeline

A HKCU Classes CLSID with InprocServer32 pointing at an AppData DLL.

## How to spot it

- An InprocServer32 under HKCU CLSID (per user wins over HKLM)
- Points at a DLL in AppData
- Loads when the hijacked COM object is invoked
- HKCU override of a normally HKLM CLSID

## Questions

```
1. Key?               HKCU CLSID InprocServer32
2. Target?             AppData\comh\evil.dll
3. Why HKCU?           Overrides HKLM
4. Loads when?          COM object used
5. Technique?          COM hijacking
```

## MITRE ATT&CK

- T1546.015 COM Hijacking

## Tools

Registry Explorer, regedit, RECmd, reg query. Open the .reg in a text editor to read the keys directly.

## Signature

Forensic Sample Blueprint | qayssarayra.com
