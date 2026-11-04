# backup-file-hunting

## Scenario
An attacker hunts for leftover backup and source files (backup.zip, db.sql, .bak) that expose code or data.

## Timeline (UTC+3)
```
11:40:00  Requests for backup/source files from 45.137.22.90
11:40:xx  Mostly 404, a large 200 on one archive
```

## How to spot it
- Requests for .zip, .sql, .tar.gz, .bak, ~ files
- Mostly 404, a large 200 when one exists
- Hunting for leftover backups and source
- One IP sweeping common names

## Questions
```
1. Attack?            Backup/source file hunting
2. Extensions?        .zip, .sql, .tar.gz, .bak, ~
3. Hit sign?          Large 200 response
4. Risk?              Code/data exposure
5. Attacker?          45.137.22.90
```

## MITRE ATT&CK
- T1595 Active Scanning

## Tools
Hand written Apache access log with real workday background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
