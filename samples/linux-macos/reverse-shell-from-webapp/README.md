# reverse-shell-from-webapp

## Scenario
A web app compromise gives the attacker a reverse shell as www-data, who then runs recon.

## Timeline
```
~10:43  www-data opens a reverse shell to 45.137.22.90:4444
```

## How to spot it
- The web user (www-data) spawning a shell
- Outbound connection to an attacker port
- Recon commands as www-data
- Web app to shell pivot

## Questions
```
1. User?               www-data
2. Shell?              Reverse to 45.137.22.90:4444
3. Origin?             Web app compromise
4. Follow up?          id, uname, download tool
5. Host?               websrv01
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 765560134145b1d19b316a2865033848845d7ef3b9bf2e29b30a26de7147c56d.zip -phidden` (or your archive tool). You get `auth.log`.
2. Read the log: `less auth.log`; for auditd use `ausearch -if auth.log`, for a journal export `journalctl --file=auth.log`.
3. Grep the indicator: `grep -Ei 'failed|accepted|sudo|useradd|/tmp/|preload|cron|wget|curl' auth.log` (narrow to this attack).
4. Confirm against the answer: The web user (www-data) spawning a shell

## MITRE ATT&CK
- T1059.004 Unix Shell
- T1505.003 Web Shell

## Tools
Hand written Linux logs with a workday of normal activity as background

## Signature
Forensic Sample Blueprint | qayssarayra.com
