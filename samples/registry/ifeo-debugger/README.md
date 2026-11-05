# Ifeo Debugger

## Scenario

An Image File Execution Options Debugger value on sethc.exe makes cmd launch when Sticky Keys is triggered, even at the lock screen.

Host: WS-FIN-07  
Evidence: `registry.reg` (exported registry keys)  
Approx hive last write: 2022-05-16 16:57

## Timeline

An IFEO subkey for sethc.exe with a Debugger value pointing at cmd.exe.

## How to spot it

- A Debugger value under an IFEO subkey
- Target is an accessibility binary (sethc.exe)
- Debugger points at cmd.exe
- Gives a SYSTEM shell from the lock screen

## Questions

```
1. Key?               IFEO sethc.exe
2. Value?              Debugger = cmd.exe
3. Trigger?            Sticky Keys at lock screen
4. Result?             SYSTEM cmd
5. Technique?          IFEO injection / accessibility abuse
```

## MITRE ATT&CK

- T1546.008 Accessibility Features
- T1546.012 IFEO Injection

## Tools

Registry Explorer, regedit, RECmd, reg query. Open the .reg in a text editor to read the keys directly.

## Signature

Forensic Sample Blueprint | qayssarayra.com
