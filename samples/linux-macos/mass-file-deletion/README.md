# mass-file-deletion

## Scenario
A destructive attacker wipes file shares and backups, and shreds logs.

## Timeline
```
~15:10  rm -rf of /srv/share and /backup, logs shredded
```

## How to spot it
- rm -rf across data and backup directories
- shred of log files
- Mass destructive deletion
- Impact stage

## Questions
```
1. Action?             Mass file deletion
2. Targets?            /srv/share, /backup, logs
3. Tools?              rm -rf, shred
4. Phase?              Impact (destruction)
5. Host?               fileserv
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x d44983e2408ebfa3ec0cd1148c22d5e3a2bdcd76412dad4488013a024ba1af0e.zip -phidden` (or your archive tool). You get `auth.log`.
2. Read the log: `less auth.log`; for auditd use `ausearch -if auth.log`, for a journal export `journalctl --file=auth.log`.
3. Grep the indicator: `grep -Ei 'failed|accepted|sudo|useradd|/tmp/|preload|cron|wget|curl' auth.log` (narrow to this attack).
4. Confirm against the answer: rm -rf across data and backup directories

## MITRE ATT&CK
- T1485 Data Destruction

## Tools
Hand written Linux logs with a workday of normal activity as background

## Signature
Forensic Sample Blueprint | qayssarayra.com
