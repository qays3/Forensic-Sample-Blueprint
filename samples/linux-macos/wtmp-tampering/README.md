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

## How to solve it

1. Extract the evidence, password `hidden`: `7z x d9335889bf28d3c6baa9a695f76564b823afc8cfe8d9219e2051f0433449f264.zip -phidden` (or your archive tool). You get `auth.log`.
2. Read the log: `less auth.log`; for auditd use `ausearch -if auth.log`, for a journal export `journalctl --file=auth.log`.
3. Grep the indicator: `grep -Ei 'failed|accepted|sudo|useradd|/tmp/|preload|cron|wget|curl' auth.log` (narrow to this attack).
4. Confirm against the answer: wtmp or btmp truncated to zero

## MITRE ATT&CK
- T1070.002 Indicator Removal: Clear Linux or Mac System Logs

## Tools
Hand written Linux logs with a workday of normal activity as background

## Signature
Forensic Sample Blueprint | qayssarayra.com
