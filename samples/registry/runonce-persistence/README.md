# Runonce Persistence

## Scenario

A RunOnce value runs a ProgramData binary at next logon, used for a one shot re execution after a reboot.

Host: WS-FIN-07  
Evidence: `registry.reg` (exported registry keys)  
Approx hive last write: 2023-01-05 18:17

## Timeline

A RunOnce value, Cleanup, pointing at C:\ProgramData\mss\upd.exe.

## How to spot it

- A RunOnce value under HKLM
- Generic name (Cleanup) hiding a ProgramData binary
- RunOnce clears itself after firing
- Pairs with a dropper that re arms it

## Questions

```
1. Key?               HKLM RunOnce
2. Value?              Cleanup
3. Path?               ProgramData\mss\upd.exe
4. Fires?              Next logon, once
5. Technique?          RunOnce persistence
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x f3f4370315b9d2caff39e52a13cf2c41fa0ccbe13001f636cecffb614b1a0a7b.zip -phidden` (or your archive tool). You get `registry.reg`.
2. Read it as text: `cat registry.reg`, or import into a throwaway hive and open in Registry Explorer / RECmd.
3. Jump to the key that matters: `grep -niA2 -E 'Run|AppInit|Userinit|Debugger|ServiceDll|Security Packages|AlwaysInstallElevated|DefaultPassword|USBSTOR' registry.reg` (narrow to this one).
4. Confirm against the answer: A RunOnce value under HKLM

## MITRE ATT&CK

- T1547.001 Registry Run Keys / Startup Folder

## Tools

Registry Explorer, regedit, RECmd, reg query. Open the .reg in a text editor to read the keys directly.

## Signature

Forensic Sample Blueprint | qayssarayra.com
