# linux-recon-commands

## Scenario
After a foothold, the attacker runs standard Linux host and network enumeration.

## Timeline
```
~10:47  Burst of recon commands as www-data
```

## How to spot it
- A burst of enumeration commands together
- id, uname, ps, ss, cat /etc/passwd, sudo -l
- Host and network discovery
- Right after initial access

## Questions
```
1. Phase?              Discovery
2. Commands?           id, uname, ps, ss, sudo -l ...
3. User?               www-data
4. Evidence?           bash_history
5. Host?               websrv01
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 162c9d96eea717c78811423f1570bc6055207c0604bf33de0e1500ad7cf5f277.zip -phidden` (or your archive tool). You get `auth.log`.
2. Read the log: `less auth.log`; for auditd use `ausearch -if auth.log`, for a journal export `journalctl --file=auth.log`.
3. Grep the indicator: `grep -Ei 'failed|accepted|sudo|useradd|/tmp/|preload|cron|wget|curl' auth.log` (narrow to this attack).
4. Confirm against the answer: A burst of enumeration commands together

## MITRE ATT&CK
- T1082 System Information Discovery
- T1033 System Owner/User Discovery

## Tools
Hand written Linux logs with a workday of normal activity as background

## Signature
Forensic Sample Blueprint | qayssarayra.com
