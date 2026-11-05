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

## MITRE ATT&CK

- T1547.001 Registry Run Keys / Startup Folder

## Tools

Registry Explorer, regedit, RECmd, reg query. Open the .reg in a text editor to read the keys directly.

## Signature

Forensic Sample Blueprint | qayssarayra.com
