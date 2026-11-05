# Netsh Helper Dll

## Scenario

A Netsh helper DLL is registered so the DLL loads every time netsh runs.

Host: WS-FIN-07  
Evidence: `registry.reg` (exported registry keys)  
Approx hive last write: 2019-12-08 16:01

## Timeline

A new value under the Netsh key naming a ProgramData DLL.

## How to spot it

- A Netsh helper value pointing at a non system DLL
- Loads whenever netsh is invoked
- Netsh runs often in scripts and by admins
- Few legit third party helpers exist

## Questions

```
1. Key?               SOFTWARE\Microsoft\Netsh
2. DLL?                ProgramData\nh\helper.dll
3. Loads when?          netsh runs
4. Why useful?          netsh runs often
5. Technique?          Netsh helper DLL
```

## MITRE ATT&CK

- T1546.007 Netsh Helper DLL

## Tools

Registry Explorer, regedit, RECmd, reg query. Open the .reg in a text editor to read the keys directly.

## Signature

Forensic Sample Blueprint | qayssarayra.com
