# ssh-password-spray

## Scenario
An attacker sprays one password across many accounts over SSH, one try each, spaced out to avoid lockout; one account (lina) succeeds.

## Timeline
```
~10:00  One SSH attempt per user from 45.137.22.90, spaced out
later   Accepted password for lina
```

## How to spot it
- One failed attempt per user, many users
- Spaced timing, not a fast burst
- Same source IP across accounts
- One eventual success

## Questions
```
1. Pattern?            One password, many users (spray)
2. Source?             45.137.22.90
3. Success account?    lina
4. Why spaced?         Avoid lockout
5. Service?            SSH
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 59997325c21310529d06acab4057d36036e5c812ca95d18c581ea6eea6ab65b6.zip -phidden` (or your archive tool). You get `auth.log`.
2. Read the log: `less auth.log`; for auditd use `ausearch -if auth.log`, for a journal export `journalctl --file=auth.log`.
3. Grep the indicator: `grep -Ei 'failed|accepted|sudo|useradd|/tmp/|preload|cron|wget|curl' auth.log` (narrow to this attack).
4. Confirm against the answer: One failed attempt per user, many users

## MITRE ATT&CK
- T1110.003 Brute Force: Password Spraying

## Tools
Hand written Linux logs with a workday of normal activity as background

## Signature
Forensic Sample Blueprint | qayssarayra.com
