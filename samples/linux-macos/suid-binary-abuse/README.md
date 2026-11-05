# suid-binary-abuse

## Scenario
An attacker enumerates SUID binaries and abuses one to escalate to root.

## Timeline
```
~13:05  SUID enumeration then abuse of a SUID binary
```

## How to spot it
- find -perm -4000 enumerating SUID files
- Execution of an unusual SUID binary
- Privilege jump to uid=0 after
- Classic SUID escalation hunt

## Questions
```
1. Enumeration?        find -perm -4000
2. Binary?             pkexec (SUID)
3. Goal?               Root
4. Evidence file?      bash_history
5. Host?               appsrv02
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 0029533d2a7707b65b05eb47b6869b4c9fa517c13c1d581d6bdfa952621e073b.zip -phidden` (or your archive tool). You get `auth.log`.
2. Read the log: `less auth.log`; for auditd use `ausearch -if auth.log`, for a journal export `journalctl --file=auth.log`.
3. Grep the indicator: `grep -Ei 'failed|accepted|sudo|useradd|/tmp/|preload|cron|wget|curl' auth.log` (narrow to this attack).
4. Confirm against the answer: find -perm -4000 enumerating SUID files

## MITRE ATT&CK
- T1548.001 Setuid and Setgid

## Tools
Hand written Linux logs with a workday of normal activity as background

## Signature
Forensic Sample Blueprint | qayssarayra.com
