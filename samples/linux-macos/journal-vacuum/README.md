# journal-vacuum

## Scenario
An attacker vacuums the systemd journal to destroy historical logs.

## Timeline
```
~15:58  journalctl --vacuum wipes old journal entries
```

## How to spot it
- journalctl --vacuum-time or --vacuum-size
- A sudden gap in journal history
- Logs shortened right after activity
- Anti forensics

## Questions
```
1. Technique?          Journal vacuum (log wipe)
2. Command?            journalctl --vacuum-time=1s
3. Effect?             Old logs destroyed
4. Category?           Defense evasion
5. Host?               appsrv02
```

## MITRE ATT&CK
- T1070.002 Indicator Removal: Clear Linux or Mac System Logs

## Tools
Hand written Linux logs with a workday of normal activity as background

## Signature
Forensic Sample Blueprint | qayssarayra.com
