# cron-persistence

## Scenario
An attacker adds a crontab entry that beacons to their server every 10 minutes for persistence.

## Timeline
```
~14:10  New crontab entry curls and runs attacker code every 10 min
```

## How to spot it
- A crontab entry fetching and running remote code
- curl|bash in a cron line
- Runs on a short interval
- Persistence for the web user

## Questions
```
1. Technique?          Cron persistence
2. User?               www-data
3. Payload?            curl ...|bash every 10 min
4. C2?                 45.137.22.90
5. Host?               websrv01
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x f92c9bd743b57756512bf8f33e37fd0e3eefebd9ceb3342d965660120f69a2dc.zip -phidden` (or your archive tool). You get `auth.log`.
2. Read the log: `less auth.log`; for auditd use `ausearch -if auth.log`, for a journal export `journalctl --file=auth.log`.
3. Grep the indicator: `grep -Ei 'failed|accepted|sudo|useradd|/tmp/|preload|cron|wget|curl' auth.log` (narrow to this attack).
4. Confirm against the answer: A crontab entry fetching and running remote code

## MITRE ATT&CK
- T1053.003 Scheduled Task/Job: Cron

## Tools
Hand written Linux logs with a workday of normal activity as background

## Signature
Forensic Sample Blueprint | qayssarayra.com
