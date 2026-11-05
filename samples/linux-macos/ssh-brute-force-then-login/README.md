# ssh-brute-force-then-login

## Scenario
An external host brute forces SSH on websrv01, failing against many invalid users, then succeeds logging in as deploy.

## Timeline
```
~11:00  Hundreds of Failed password lines from 45.137.22.90
later   Accepted password for deploy, session opened
```

## How to spot it
- Many Failed password lines from one external IP
- Invalid user names tried in sequence
- One Accepted password ends the burst
- Session opened for the cracked account

## Questions
```
1. Attacker IP?        45.137.22.90
2. Service?            SSH (sshd)
3. Account cracked?    deploy
4. Attempts?           ~150 failed then 1 success
5. Host?               websrv01
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 25a909f3dbc8f51e05b4d3fbf9e69bea5412b30036701006014ea0a8bdafe584.zip -phidden` (or your archive tool). You get `auth.log`.
2. Read the log: `less auth.log`; for auditd use `ausearch -if auth.log`, for a journal export `journalctl --file=auth.log`.
3. Grep the indicator: `grep -Ei 'failed|accepted|sudo|useradd|/tmp/|preload|cron|wget|curl' auth.log` (narrow to this attack).
4. Confirm against the answer: Many Failed password lines from one external IP

## MITRE ATT&CK
- T1110.001 Brute Force: Password Guessing
- T1021.004 Remote Services: SSH

## Tools
Hand written Linux logs with a workday of normal activity as background

## Signature
Forensic Sample Blueprint | qayssarayra.com
