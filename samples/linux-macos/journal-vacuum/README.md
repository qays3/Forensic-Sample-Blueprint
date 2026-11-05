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

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 3a1dc8b214c88198822acea94a1a93a146a43c109d1883e6760be2074e8939e7.zip -phidden` (or your archive tool). You get `auth.log`.
2. Read the log: `less auth.log`; for auditd use `ausearch -if auth.log`, for a journal export `journalctl --file=auth.log`.
3. Grep the indicator: `grep -Ei 'failed|accepted|sudo|useradd|/tmp/|preload|cron|wget|curl' auth.log` (narrow to this attack).
4. Confirm against the answer: journalctl --vacuum-time or --vacuum-size

## MITRE ATT&CK
- T1070.002 Indicator Removal: Clear Linux or Mac System Logs

## Tools
Hand written Linux logs with a workday of normal activity as background

## Signature
Forensic Sample Blueprint | qayssarayra.com
