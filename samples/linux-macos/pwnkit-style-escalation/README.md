# pwnkit-style-escalation

## Scenario
An attacker runs a PwnKit (CVE-2021-4034) style local exploit against pkexec to get root.

## Timeline
```
~13:15  pkexec exploit compiled and run, euid becomes 0
```

## How to spot it
- pkexec segfaults or odd GCONV_PATH use
- A local exploit binary compiled and run
- euid jumps to 0 via pkexec
- Known PwnKit pattern

## Questions
```
1. Target binary?      pkexec
2. Exploit?            PwnKit style (CVE-2021-4034)
3. Result?             euid=0
4. Evidence?           bash_history + audit
5. Host?               appsrv02
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 4e95225ee80f9021826eede6a04ea0b0d8b6f938fef9a69c615eb12b1d3f9029.zip -phidden` (or your archive tool). You get `auth.log`.
2. Read the log: `less auth.log`; for auditd use `ausearch -if auth.log`, for a journal export `journalctl --file=auth.log`.
3. Grep the indicator: `grep -Ei 'failed|accepted|sudo|useradd|/tmp/|preload|cron|wget|curl' auth.log` (narrow to this attack).
4. Confirm against the answer: pkexec segfaults or odd GCONV_PATH use

## MITRE ATT&CK
- T1068 Exploitation for Privilege Escalation

## Tools
Hand written Linux logs with a workday of normal activity as background

## Signature
Forensic Sample Blueprint | qayssarayra.com
