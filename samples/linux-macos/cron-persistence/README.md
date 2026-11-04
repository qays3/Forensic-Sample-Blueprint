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

## MITRE ATT&CK
- T1053.003 Scheduled Task/Job: Cron

## Tools
Hand written Linux logs with a workday of normal activity as background

## Signature
Forensic Sample Blueprint | qayssarayra.com
