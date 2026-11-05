# Office Test Key

## Scenario

The Office test Special Perf key loads a DLL into every Office application at startup, a quiet Office persistence.

Host: WS-FIN-07  
Evidence: `registry.reg` (exported registry keys)  
Approx hive last write: 2022-04-19 15:41

## Timeline

An Office test\Special\Perf key with a default value pointing at an AppData DLL.

## How to spot it

- The rarely used Office test\Special\Perf key present
- Default value pointing at a DLL
- Loads into Word, Excel and others
- Key does not exist on a clean host

## Questions

```
1. Key?               Office test\Special\Perf
2. DLL?                AppData\ot\addin.dll
3. Loads into?          Every Office app
4. Normal?             Key absent by default
5. Technique?          Office test key persistence
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x fd9b9aab006fc510922ca73ad83f9c1dd0b3fcea43fe0d75ea68f115c1093bc0.zip -phidden` (or your archive tool). You get `registry.reg`.
2. Read it as text: `cat registry.reg`, or import into a throwaway hive and open in Registry Explorer / RECmd.
3. Jump to the key that matters: `grep -niA2 -E 'Run|AppInit|Userinit|Debugger|ServiceDll|Security Packages|AlwaysInstallElevated|DefaultPassword|USBSTOR' registry.reg` (narrow to this one).
4. Confirm against the answer: The rarely used Office test\Special\Perf key present

## MITRE ATT&CK

- T1137.002 Office Test

## Tools

Registry Explorer, regedit, RECmd, reg query. Open the .reg in a text editor to read the keys directly.

## Signature

Forensic Sample Blueprint | qayssarayra.com
