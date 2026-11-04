# file-upload-webshell

## Scenario
An attacker uploads a PHP webshell through an upload form, then requests it from the uploads folder.

## Timeline (UTC+3)
```
14:20:00  POST /upload stores shell.php
14:20:xx  GET /uploads/shell.php confirms it runs
```

## How to spot it
- A POST to an upload endpoint followed by a GET of a .php in uploads
- The uploaded file has a script extension
- The follow up GET returns 200, shell is live
- Upload then execute pattern

## Questions
```
1. Upload endpoint?   /upload
2. Shell path?        /uploads/shell.php
3. Pattern?           Upload then request the file
4. File type?         PHP webshell
5. Confirm?           200 on the shell
```

## MITRE ATT&CK
- T1505.003 Server Software Component: Web Shell

## Tools
Hand written Apache access log with real workday background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
