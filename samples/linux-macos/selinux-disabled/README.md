# selinux-disabled

## Scenario
An attacker sets SELinux to permissive to remove mandatory access control protections.

## Timeline
```
~15:57  setenforce 0 and config changed to permissive
```

## How to spot it
- setenforce 0 switching SELinux off
- Edit of /etc/selinux/config
- Protections dropped before further action
- Defense evasion

## Questions
```
1. Technique?          Disable SELinux
2. Command?            setenforce 0
3. Persistent change?  /etc/selinux/config
4. Category?           Defense evasion
5. Host?               dbsrv01
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x f57f23990842a4b087a468fa9f1b21a045a81954eef9c0fb654caba9e65bd479.zip -phidden` (or your archive tool). You get `auth.log`.
2. Read the log: `less auth.log`; for auditd use `ausearch -if auth.log`, for a journal export `journalctl --file=auth.log`.
3. Grep the indicator: `grep -Ei 'failed|accepted|sudo|useradd|/tmp/|preload|cron|wget|curl' auth.log` (narrow to this attack).
4. Confirm against the answer: setenforce 0 switching SELinux off

## MITRE ATT&CK
- T1562.001 Impair Defenses: Disable or Modify Tools

## Tools
Hand written Linux logs with a workday of normal activity as background

## Signature
Forensic Sample Blueprint | qayssarayra.com
