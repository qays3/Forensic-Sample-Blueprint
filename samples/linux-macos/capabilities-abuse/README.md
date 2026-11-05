# capabilities-abuse

## Scenario
An attacker finds a binary with cap_setuid and uses it to become root.

## Timeline
```
~13:10  getcap enumeration then python3 setuid(0) to root
```

## How to spot it
- getcap -r enumerating file capabilities
- A binary with cap_setuid abused
- python3 os.setuid(0) spawning a shell
- Root without sudo or SUID

## Questions
```
1. Enumeration?        getcap -r /
2. Capability?          cap_setuid
3. Binary?              python3
4. Result?              Root shell
5. Host?                appsrv02
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 8670d62aa86cb94c2a75637e449d752bce3fbb073f227bcb884ff9bb51e205cf.zip -phidden` (or your archive tool). You get `auth.log`.
2. Read the log: `less auth.log`; for auditd use `ausearch -if auth.log`, for a journal export `journalctl --file=auth.log`.
3. Grep the indicator: `grep -Ei 'failed|accepted|sudo|useradd|/tmp/|preload|cron|wget|curl' auth.log` (narrow to this attack).
4. Confirm against the answer: getcap -r enumerating file capabilities

## MITRE ATT&CK
- T1548 Abuse Elevation Control Mechanism

## Tools
Hand written Linux logs with a workday of normal activity as background

## Signature
Forensic Sample Blueprint | qayssarayra.com
