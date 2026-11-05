# Run Key Persistence

## Scenario

A Run value points at a binary in a user AppData folder, a classic autostart for a payload that survives reboot.

Host: WS-FIN-07  
Evidence: `registry.reg` (exported registry keys)  
Approx hive last write: 2019-07-09 17:46

## Timeline

Legit Run values plus one extra, WindowsUpdate, pointing at AppData\Roaming\svchost.exe.

## How to spot it

- A Run value naming a system process but living in AppData
- Path under Roaming, not System32
- Name chosen to look like Windows Update
- Everything else in the key is normal

## Questions

```
1. Artifact?          HKCU Run value
2. Which one?          WindowsUpdate
3. Path?               AppData\Roaming\svchost.exe
4. Why suspicious?     System name, user path
5. Technique?          Run key persistence
```

## MITRE ATT&CK

- T1547.001 Registry Run Keys / Startup Folder

## Tools

Registry Explorer, regedit, RECmd, reg query. Open the .reg in a text editor to read the keys directly.

## Signature

Forensic Sample Blueprint | qayssarayra.com
