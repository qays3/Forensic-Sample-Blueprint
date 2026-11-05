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

## How to solve it

1. Extract the evidence, password `hidden`: `7z x b07a278d2e1f3e7d5fa38fd301f405ee968861d2e5fc4991b78257bb6a55f8ad.zip -phidden` (or your archive tool). You get `registry.reg`.
2. Read it as text: `cat registry.reg`, or import into a throwaway hive and open in Registry Explorer / RECmd.
3. Jump to the key that matters: `grep -niA2 -E 'Run|AppInit|Userinit|Debugger|ServiceDll|Security Packages|AlwaysInstallElevated|DefaultPassword|USBSTOR' registry.reg` (narrow to this one).
4. Confirm against the answer: SCRNSAVE.EXE pointing outside System32

## MITRE ATT&CK

- T1546.002 Screensaver

## Tools

Registry Explorer, regedit, RECmd, reg query. Open the .reg in a text editor to read the keys directly.

## Signature

Forensic Sample Blueprint | qayssarayra.com
