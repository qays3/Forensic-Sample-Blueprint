# pam-backdoor

## Scenario
An attacker replaces a PAM module with a backdoored one that accepts a magic password for any account.

## Timeline
```
~14:26  Backdoored pam_unix.so installed, root login accepted with magic password
```

## How to spot it
- A replaced PAM module (pam_unix.so)
- Logins accepted that should not be
- A magic password working for any user
- Hard to spot without file integrity checks

## Questions
```
1. Module?             pam_unix.so
2. Technique?          PAM backdoor
3. Effect?             Magic password for any account
4. Evidence?           Modified module, odd root login
5. Host?               jumphost
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 30f8a65eee53e80beb28bd566b4b5076cfe98e662d9926355ea42406f627625a.zip -phidden` (or your archive tool). You get `auth.log`.
2. Read the log: `less auth.log`; for auditd use `ausearch -if auth.log`, for a journal export `journalctl --file=auth.log`.
3. Grep the indicator: `grep -Ei 'failed|accepted|sudo|useradd|/tmp/|preload|cron|wget|curl' auth.log` (narrow to this attack).
4. Confirm against the answer: A replaced PAM module (pam_unix.so)

## MITRE ATT&CK
- T1556.003 Modify Authentication Process: Pluggable Authentication Modules

## Tools
Hand written Linux logs with a workday of normal activity as background

## Signature
Forensic Sample Blueprint | qayssarayra.com
