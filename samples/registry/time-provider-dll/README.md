# Time Provider Dll

## Scenario

A new W32Time TimeProvider loads an attacker DLL under the time service at boot.

Host: WS-FIN-07  
Evidence: `registry.reg` (exported registry keys)  
Approx hive last write: 2019-03-22 18:01

## Timeline

A new TimeProviders subkey with a DllName in ProgramData and Enabled set.

## How to spot it

- A new TimeProviders subkey
- DllName outside System32
- Enabled set to 1
- Loaded by the time service at startup

## Questions

```
1. Key?               W32Time\TimeProviders
2. DLL?                ProgramData\tp\time.dll
3. Enabled?            1
4. Loaded by?           Time service at boot
5. Technique?          Time provider DLL
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 4c7f0538a398b4a5fd5cbda7141b1f5497e6de48b8cb5d1b46c970af5181d433.zip -phidden` (or your archive tool). You get `registry.reg`.
2. Read it as text: `cat registry.reg`, or import into a throwaway hive and open in Registry Explorer / RECmd.
3. Jump to the key that matters: `grep -niA2 -E 'Run|AppInit|Userinit|Debugger|ServiceDll|Security Packages|AlwaysInstallElevated|DefaultPassword|USBSTOR' registry.reg` (narrow to this one).
4. Confirm against the answer: A new TimeProviders subkey

## MITRE ATT&CK

- T1547.003 Time Providers

## Tools

Registry Explorer, regedit, RECmd, reg query. Open the .reg in a text editor to read the keys directly.

## Signature

Forensic Sample Blueprint | qayssarayra.com
