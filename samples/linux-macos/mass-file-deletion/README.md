# mass-file-deletion

## Scenario
A destructive attacker wipes file shares and backups, and shreds logs.

## Timeline
```
~15:10  rm -rf of /srv/share and /backup, logs shredded
```

## How to spot it
- rm -rf across data and backup directories
- shred of log files
- Mass destructive deletion
- Impact stage

## Questions
```
1. Action?             Mass file deletion
2. Targets?            /srv/share, /backup, logs
3. Tools?              rm -rf, shred
4. Phase?              Impact (destruction)
5. Host?               fileserv
```

## MITRE ATT&CK
- T1485 Data Destruction

## Tools
Hand written Linux logs with a workday of normal activity as background

## Signature
Forensic Sample Blueprint | qayssarayra.com
