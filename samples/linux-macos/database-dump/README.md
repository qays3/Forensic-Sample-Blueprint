# database-dump

## Scenario
An attacker dumps the whole database to a file for theft.

## Timeline
```
~15:05  mysqldump --all-databases writes /tmp/.db.sql
```

## How to spot it
- mysqldump of all databases
- A large .sql or .sql.gz in /tmp
- Database export by an odd user
- Collection before exfil

## Questions
```
1. Action?             Full database dump
2. Tool?               mysqldump --all-databases
3. Output?             /tmp/.db.sql.gz
4. Phase?              Collection
5. Host?               dbsrv01
```

## MITRE ATT&CK
- T1005 Data from Local System

## Tools
Hand written Linux logs with a workday of normal activity as background

## Signature
Forensic Sample Blueprint | qayssarayra.com
