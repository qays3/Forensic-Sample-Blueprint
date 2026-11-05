# Appinit Dlls

## Scenario

AppInit_DLLs names a DLL that loads into most processes using user32.dll, and LoadAppInit_DLLs is enabled.

Host: WS-FIN-07  
Evidence: `registry.reg` (exported registry keys)  
Approx hive last write: 2019-11-03 19:38

## Timeline

AppInit_DLLs points at a ProgramData DLL and LoadAppInit_DLLs is set to 1.

## How to spot it

- AppInit_DLLs holding a non empty path
- LoadAppInit_DLLs set to 1
- DLL loads into many processes at once
- Usually empty on a clean host

## Questions

```
1. Values?            AppInit_DLLs, LoadAppInit_DLLs
2. DLL?                ProgramData\appinit\hook.dll
3. Enabled?            LoadAppInit_DLLs = 1
4. Effect?             DLL into many processes
5. Technique?          AppInit DLL injection
```

## MITRE ATT&CK

- T1546.010 AppInit DLLs

## Tools

Registry Explorer, regedit, RECmd, reg query. Open the .reg in a text editor to read the keys directly.

## Signature

Forensic Sample Blueprint | qayssarayra.com
