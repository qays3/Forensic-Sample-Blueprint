# auditd-disabled

## Scenario
An attacker disables auditd to stop security logging before acting.

## Timeline
```
~15:56  auditctl -D clears rules, auditd stopped
```

## How to spot it
- auditctl -D or -e 0 clearing/disabling audit
- auditd service stopped
- Logging goes dark
- Anti forensics / defense evasion

## Questions
```
1. Technique?          Disable auditd
2. Commands?           auditctl -D, stop auditd
3. Effect?             No audit logging
4. Category?           Defense evasion
5. Host?               dbsrv01
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 01a82ad1a99f27d3ae85db241a89e96bff64c2db51f0a9be136309b5ffca5bf2.zip -phidden` (or your archive tool). You get `auth.log`.
2. Read the log: `less auth.log`; for auditd use `ausearch -if auth.log`, for a journal export `journalctl --file=auth.log`.
3. Grep the indicator: `grep -Ei 'failed|accepted|sudo|useradd|/tmp/|preload|cron|wget|curl' auth.log` (narrow to this attack).
4. Confirm against the answer: auditctl -D or -e 0 clearing/disabling audit

## MITRE ATT&CK
- T1562.001 Impair Defenses: Disable or Modify Tools

## Tools
Hand written Linux logs with a workday of normal activity as background

## Signature
Forensic Sample Blueprint | qayssarayra.com
