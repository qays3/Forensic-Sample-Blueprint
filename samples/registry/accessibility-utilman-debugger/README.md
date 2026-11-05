# Accessibility Utilman Debugger

## Scenario

A Debugger value on utilman.exe gives a SYSTEM command prompt from the lock screen Ease of Access button.

Host: WS-FIN-07  
Evidence: `registry.reg` (exported registry keys)  
Approx hive last write: 2022-12-15 11:49

## Timeline

An IFEO subkey for utilman.exe with Debugger set to cmd.exe.

## How to spot it

- An IFEO Debugger on utilman.exe
- Triggered by the Ease of Access button pre logon
- cmd runs as SYSTEM
- No credentials needed

## Questions

```
1. Key?               IFEO utilman.exe
2. Value?              Debugger = cmd.exe
3. Trigger?            Ease of Access, lock screen
4. Context?            SYSTEM
5. Technique?          Accessibility abuse
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 7a7e1582d9f7a8ddd16a4e78fd2687bcafbe53eddac493a5597ca67691c05e38.zip -phidden` (or your archive tool). You get `registry.reg`.
2. Read it as text: `cat registry.reg`, or import into a throwaway hive and open in Registry Explorer / RECmd.
3. Jump to the key that matters: `grep -niA2 -E 'Run|AppInit|Userinit|Debugger|ServiceDll|Security Packages|AlwaysInstallElevated|DefaultPassword|USBSTOR' registry.reg` (narrow to this one).
4. Confirm against the answer: An IFEO Debugger on utilman.exe

## MITRE ATT&CK

- T1546.008 Accessibility Features

## Tools

Registry Explorer, regedit, RECmd, reg query. Open the .reg in a text editor to read the keys directly.

## Signature

Forensic Sample Blueprint | qayssarayra.com
