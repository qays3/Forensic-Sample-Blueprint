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

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 414acde7331c5cf6ede1e02f59da314900d17417840d1781936166ec86b27240.zip -phidden` (or your archive tool). You get `registry.reg`.
2. Read it as text: `cat registry.reg`, or import into a throwaway hive and open in Registry Explorer / RECmd.
3. Jump to the key that matters: `grep -niA2 -E 'Run|AppInit|Userinit|Debugger|ServiceDll|Security Packages|AlwaysInstallElevated|DefaultPassword|USBSTOR' registry.reg` (narrow to this one).
4. Confirm against the answer: An InprocServer32 under HKCU CLSID (per user wins over HKLM)

## MITRE ATT&CK

- T1546.015 COM Hijacking

## Tools

Registry Explorer, regedit, RECmd, reg query. Open the .reg in a text editor to read the keys directly.

## Signature

Forensic Sample Blueprint | qayssarayra.com
