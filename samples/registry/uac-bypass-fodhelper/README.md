# Uac Bypass Fodhelper

## Scenario

A hijacked ms-settings open command with an empty DelegateExecute makes fodhelper.exe (or computerdefaults/sdclt) run a payload elevated without a UAC prompt.

Host: WS-FIN-07  
Evidence: `registry.reg` (exported registry keys)  
Approx hive last write: 2024-04-21 09:54

## Timeline

A HKCU Classes ms-settings\shell\open\command with a payload and an empty DelegateExecute.

## How to spot it

- A HKCU ms-settings open command override
- An empty DelegateExecute value
- Abused by fodhelper / computerdefaults / sdclt
- Elevates with no prompt

## Questions

```
1. Key?               HKCU ms-settings open command
2. Extra value?         DelegateExecute (empty)
3. Abuses?             fodhelper auto elevate
4. Result?             Elevated, no prompt
5. Technique?          UAC bypass (fodhelper)
```

## MITRE ATT&CK

- T1548.002 Bypass UAC

## Tools

Registry Explorer, regedit, RECmd, reg query. Open the .reg in a text editor to read the keys directly.

## Signature

Forensic Sample Blueprint | qayssarayra.com
