# Uac Bypass Eventvwr

## Scenario

A hijacked mscfile open command makes the auto elevating eventvwr.exe launch an attacker binary with high integrity, bypassing UAC.

Host: WS-FIN-07  
Evidence: `registry.reg` (exported registry keys)  
Approx hive last write: 2022-04-21 17:58

## Timeline

A HKCU Classes mscfile\shell\open\command pointing at a ProgramData binary.

## How to spot it

- A HKCU mscfile open command override
- Abused by auto elevating eventvwr.exe
- Runs the payload high integrity, no prompt
- HKCU override beats HKLM default

## Questions

```
1. Key?               HKCU mscfile open command
2. Abuses?             eventvwr.exe auto elevate
3. Payload?            ProgramData\u\p.exe
4. Result?             High integrity, no UAC
5. Technique?          UAC bypass (eventvwr)
```

## MITRE ATT&CK

- T1548.002 Bypass UAC

## Tools

Registry Explorer, regedit, RECmd, reg query. Open the .reg in a text editor to read the keys directly.

## Signature

Forensic Sample Blueprint | qayssarayra.com
