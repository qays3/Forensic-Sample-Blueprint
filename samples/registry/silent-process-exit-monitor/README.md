# Silent Process Exit Monitor

## Scenario

A SilentProcessExit monitor on lsass.exe runs an attacker process (or writes a dump) whenever lsass exits, used for persistence and LSASS dumping.

Host: WS-FIN-07  
Evidence: `registry.reg` (exported registry keys)  
Approx hive last write: 2022-09-28 14:07

## Timeline

A SilentProcessExit subkey for lsass.exe with a MonitorProcess and a dump folder.

## How to spot it

- A SilentProcessExit subkey naming lsass.exe
- A MonitorProcess pointing at a ProgramData binary
- GlobalFlag and DumpType set for dumping
- Fires on process exit, both persistence and cred theft

## Questions

```
1. Key?               SilentProcessExit lsass.exe
2. Values?             MonitorProcess, LocalDumpFolder
3. Fires when?          lsass exits
4. Dual use?           Persistence + LSASS dump
5. Technique?          SilentProcessExit abuse
```

## MITRE ATT&CK

- T1546.012 IFEO Injection
- T1003.001 LSASS Memory

## Tools

Registry Explorer, regedit, RECmd, reg query. Open the .reg in a text editor to read the keys directly.

## Signature

Forensic Sample Blueprint | qayssarayra.com
