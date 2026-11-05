# writable-cron-hijack

## Scenario
An attacker writes to a world writable cron file so a root cron job runs their script.

## Timeline
```
~14:00  Root cron runs /tmp/.update.sh planted by the attacker
```

## How to spot it
- A root cron running a script from /tmp
- A new or modified file in /etc/cron.d
- Hidden script name (dot prefix)
- Root execution of attacker content

## Questions
```
1. Technique?          Writable cron hijack
2. Cron file?          /etc/cron.d/update
3. Payload?            /tmp/.update.sh (root)
4. Persistence?        Runs every minute
5. Host?               dbsrv01
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 4ea18e33e44a7ffef501879a8c72eb0aeb09da50e9a1e3703d22bae7ad461d6c.zip -phidden` (or your archive tool). You get `auth.log`.
2. Read the log: `less auth.log`; for auditd use `ausearch -if auth.log`, for a journal export `journalctl --file=auth.log`.
3. Grep the indicator: `grep -Ei 'failed|accepted|sudo|useradd|/tmp/|preload|cron|wget|curl' auth.log` (narrow to this attack).
4. Confirm against the answer: A root cron running a script from /tmp

## MITRE ATT&CK
- T1053.003 Scheduled Task/Job: Cron

## Tools
Hand written Linux logs with a workday of normal activity as background

## Signature
Forensic Sample Blueprint | qayssarayra.com
