# suid-binary-abuse

## Scenario
An attacker enumerates SUID binaries and abuses one to escalate to root.

## Timeline
```
~13:05  SUID enumeration then abuse of a SUID binary
```

## How to spot it
- find -perm -4000 enumerating SUID files
- Execution of an unusual SUID binary
- Privilege jump to uid=0 after
- Classic SUID escalation hunt

## Questions
```
1. Enumeration?        find -perm -4000
2. Binary?             pkexec (SUID)
3. Goal?               Root
4. Evidence file?      bash_history
5. Host?               appsrv02
```

## MITRE ATT&CK
- T1548.001 Setuid and Setgid

## Tools
Hand written Linux logs with a workday of normal activity as background

## Signature
Forensic Sample Blueprint | qayssarayra.com
