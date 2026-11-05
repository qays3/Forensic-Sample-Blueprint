# database-dump

## Scenario
An attacker dumps the whole database to a file for theft.

## Timeline
```
~15:05  mysqldump --all-databases writes /tmp/.db.sql
```

## How to spot it
- mysqldump of all databases
- A large .sql or .sql.gz in /tmp
- Database export by an odd user
- Collection before exfil

## Questions
```
1. Action?             Full database dump
2. Tool?               mysqldump --all-databases
3. Output?             /tmp/.db.sql.gz
4. Phase?              Collection
5. Host?               dbsrv01
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 22157442e1d87d876d2268ba6e0c101e19f360260a775452e6218391efa1dfc7.zip -phidden` (or your archive tool). You get `auth.log`.
2. Read the log: `less auth.log`; for auditd use `ausearch -if auth.log`, for a journal export `journalctl --file=auth.log`.
3. Grep the indicator: `grep -Ei 'failed|accepted|sudo|useradd|/tmp/|preload|cron|wget|curl' auth.log` (narrow to this attack).
4. Confirm against the answer: mysqldump of all databases

## MITRE ATT&CK
- T1005 Data from Local System

## Tools
Hand written Linux logs with a workday of normal activity as background

## Signature
Forensic Sample Blueprint | qayssarayra.com
