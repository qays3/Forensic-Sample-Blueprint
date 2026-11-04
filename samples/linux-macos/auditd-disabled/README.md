# auditd-disabled

## Scenario
An attacker disables auditd to stop security logging before acting.

## Timeline
```
~15:56  auditctl -D clears rules, auditd stopped
```

## How to spot it
- auditctl -D or -e 0 clearing/disabling audit
- auditd service stopped
- Logging goes dark
- Anti forensics / defense evasion

## Questions
```
1. Technique?          Disable auditd
2. Commands?           auditctl -D, stop auditd
3. Effect?             No audit logging
4. Category?           Defense evasion
5. Host?               dbsrv01
```

## MITRE ATT&CK
- T1562.001 Impair Defenses: Disable or Modify Tools

## Tools
Hand written Linux logs with a workday of normal activity as background

## Signature
Forensic Sample Blueprint | qayssarayra.com
