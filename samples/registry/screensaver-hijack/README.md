# Screensaver Hijack

## Scenario

The screensaver is pointed at an attacker .scr binary, which runs when the machine idles.

Host: WS-FIN-07  
Evidence: `registry.reg` (exported registry keys)  
Approx hive last write: 2022-10-16 14:08

## Timeline

SCRNSAVE.EXE set to an AppData .scr with a short idle timeout.

## How to spot it

- SCRNSAVE.EXE pointing outside System32
- A short ScreenSaveTimeOut
- Runs on idle with the user's rights
- Uncommon autostart location

## Questions

```
1. Value?             SCRNSAVE.EXE
2. Path?               AppData\ss\run.scr
3. Fires on?            Idle timeout
4. Rights?             The user's
5. Technique?          Screensaver persistence
```

## MITRE ATT&CK

- T1546.002 Screensaver

## Tools

Registry Explorer, regedit, RECmd, reg query. Open the .reg in a text editor to read the keys directly.

## Signature

Forensic Sample Blueprint | qayssarayra.com
