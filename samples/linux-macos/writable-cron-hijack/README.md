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

## MITRE ATT&CK
- T1053.003 Scheduled Task/Job: Cron

## Tools
Hand written Linux logs with a workday of normal activity as background

## Signature
Forensic Sample Blueprint | qayssarayra.com
