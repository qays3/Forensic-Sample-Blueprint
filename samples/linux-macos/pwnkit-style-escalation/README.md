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

## MITRE ATT&CK
- T1068 Exploitation for Privilege Escalation

## Tools
Hand written Linux logs with a workday of normal activity as background

## Signature
Forensic Sample Blueprint | qayssarayra.com
