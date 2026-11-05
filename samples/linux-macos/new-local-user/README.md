# new-local-user

## Scenario
An attacker creates a hidden UID 0 account (svc-sync) for persistent root access.

## Timeline
```
~14:28  useradd creates svc-sync with UID 0 (root equivalent)
```

## How to spot it
- A new account created with UID 0
- useradd -o -u 0 (duplicate root)
- A service looking name to blend in
- Backdoor root account

## Questions
```
1. Account?            svc-sync
2. UID?                0 (root)
3. Flag?               useradd -o -u 0
4. Why?                Persistent root
5. Host?               appsrv02
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 3a503536ef46e514820d6143c629856f0a3624a61168edcff26d07a1894cf9a3.zip -phidden` (or your archive tool). You get `auth.log`.
2. Read the log: `less auth.log`; for auditd use `ausearch -if auth.log`, for a journal export `journalctl --file=auth.log`.
3. Grep the indicator: `grep -Ei 'failed|accepted|sudo|useradd|/tmp/|preload|cron|wget|curl' auth.log` (narrow to this attack).
4. Confirm against the answer: A new account created with UID 0

## MITRE ATT&CK
- T1136.001 Create Account: Local Account

## Tools
Hand written Linux logs with a workday of normal activity as background

## Signature
Forensic Sample Blueprint | qayssarayra.com
