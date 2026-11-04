# wtmp-tampering

## Scenario
An attacker truncates wtmp/btmp to erase login records.

## Timeline
```
~15:59  wtmp and btmp truncated, login history erased
```

## How to spot it
- wtmp or btmp truncated to zero
- last showing a suspiciously short history
- Login records destroyed
- Anti forensics

## Questions
```
1. Files?              /var/log/wtmp, btmp
2. Action?             Truncated to empty
3. Effect?             Login history erased
4. Category?           Defense evasion
5. Host?               jumphost
```

## MITRE ATT&CK
- T1070.002 Indicator Removal: Clear Linux or Mac System Logs

## Tools
Hand written Linux logs with a workday of normal activity as background

## Signature
Forensic Sample Blueprint | qayssarayra.com
