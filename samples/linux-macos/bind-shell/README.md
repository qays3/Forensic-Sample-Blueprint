# bind-shell

## Scenario
An attacker opens a bind shell, listening on a port so they can connect in and get a shell.

## Timeline
```
~14:30  A bind shell listener opens on port 9001
```

## How to spot it
- A new listener on an odd high port
- nc with -e /bin/bash
- Inbound connections to that port follow
- Bind (not reverse) shell

## Questions
```
1. Type?               Bind shell
2. Port?               9001
3. Tool?               nc -e /bin/bash
4. Direction?          Attacker connects in
5. Host?               appsrv02
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x d1965bb18f8ad9e6792db9a6ca4e7938723171f4d25e6175f60e4602c52384da.zip -phidden` (or your archive tool). You get `auth.log`.
2. Read the log: `less auth.log`; for auditd use `ausearch -if auth.log`, for a journal export `journalctl --file=auth.log`.
3. Grep the indicator: `grep -Ei 'failed|accepted|sudo|useradd|/tmp/|preload|cron|wget|curl' auth.log` (narrow to this attack).
4. Confirm against the answer: A new listener on an odd high port

## MITRE ATT&CK
- T1059.004 Unix Shell

## Tools
Hand written Linux logs with a workday of normal activity as background

## Signature
Forensic Sample Blueprint | qayssarayra.com
