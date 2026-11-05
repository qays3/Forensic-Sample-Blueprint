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

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 0c98e89813c3988ac106375619197406d0c0c0906c1c6596d299c2606855f218.zip -phidden` (or your archive tool). You get `auth.log`.
2. Read the log: `less auth.log`; for auditd use `ausearch -if auth.log`, for a journal export `journalctl --file=auth.log`.
3. Grep the indicator: `grep -Ei 'failed|accepted|sudo|useradd|/tmp/|preload|cron|wget|curl' auth.log` (narrow to this attack).
4. Confirm against the answer: A new .timer unit in /etc/systemd/system

## MITRE ATT&CK
- T1053.006 Scheduled Task/Job: Systemd Timers

## Tools
Hand written Linux logs with a workday of normal activity as background

## Signature
Forensic Sample Blueprint | qayssarayra.com
