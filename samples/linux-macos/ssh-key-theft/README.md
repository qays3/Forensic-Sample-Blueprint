# ssh-key-theft

## Scenario
An attacker hunts for and steals SSH private keys to move to other hosts.

## Timeline
```
~14:37  Private keys found and copied from user home directories
```

## How to spot it
- find for id_rsa across home directories
- Reads and copies of private keys
- Keys staged in /tmp
- Enables lateral movement

## Questions
```
1. Target?             SSH private keys (id_rsa)
2. Where?              user .ssh folders
3. Action?             Copy to /tmp
4. Use?                Lateral movement
5. Host?               jumphost
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 933e33116ac6fb9daf171e342a187f3e111c43293c23d5ddbe39a9556f6a2810.zip -phidden` (or your archive tool). You get `auth.log`.
2. Read the log: `less auth.log`; for auditd use `ausearch -if auth.log`, for a journal export `journalctl --file=auth.log`.
3. Grep the indicator: `grep -Ei 'failed|accepted|sudo|useradd|/tmp/|preload|cron|wget|curl' auth.log` (narrow to this attack).
4. Confirm against the answer: find for id_rsa across home directories

## MITRE ATT&CK
- T1552.004 Unsecured Credentials: Private Keys

## Tools
Hand written Linux logs with a workday of normal activity as background

## Signature
Forensic Sample Blueprint | qayssarayra.com
