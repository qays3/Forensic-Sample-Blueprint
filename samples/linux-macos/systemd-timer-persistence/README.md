# systemd-timer-persistence

## Scenario
An attacker uses a systemd timer to run a payload on a schedule, a stealthier alternative to cron.

## Timeline
```
~14:20  New sync-check.timer triggers a service every 15 minutes
```

## How to spot it
- A new .timer unit in /etc/systemd/system
- A paired service the timer runs
- Scheduled execution without cron
- Stealthier persistence

## Questions
```
1. Technique?          systemd timer persistence
2. Timer?              sync-check.timer
3. Interval?           15 minutes
4. Why timers?         Quieter than cron
5. Host?               dbsrv01
```

## MITRE ATT&CK
- T1053.006 Scheduled Task/Job: Systemd Timers

## Tools
Hand written Linux logs with a workday of normal activity as background

## Signature
Forensic Sample Blueprint | qayssarayra.com
