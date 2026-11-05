# bashrc-persistence

## Scenario
An attacker appends a reverse shell line to .bashrc so it runs on every login.

## Timeline
```
~14:23  Reverse shell line added to ~/.bashrc
```

## How to spot it
- A reverse shell appended to .bashrc or .profile
- Runs on each interactive login
- Outbound to an attacker IP on login
- Shell init file modified

## Questions
```
1. File?               ~/.bashrc
2. Payload?            bash reverse shell to 45.137.22.90:4444
3. Trigger?            Every login
4. Technique?          Shell init persistence
5. User?               www-data
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x da1a7e615da2ba791cdaed2f5a9dab3401a49bc4987423bc633c32e60b60b244.zip -phidden` (or your archive tool). You get `auth.log`.
2. Read the log: `less auth.log`; for auditd use `ausearch -if auth.log`, for a journal export `journalctl --file=auth.log`.
3. Grep the indicator: `grep -Ei 'failed|accepted|sudo|useradd|/tmp/|preload|cron|wget|curl' auth.log` (narrow to this attack).
4. Confirm against the answer: A reverse shell appended to .bashrc or .profile

## MITRE ATT&CK
- T1546.004 Event Triggered Execution: Unix Shell Configuration Modification

## Tools
Hand written Linux logs with a workday of normal activity as background

## Signature
Forensic Sample Blueprint | qayssarayra.com
