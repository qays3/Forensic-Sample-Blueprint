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

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 01a79cabde21ad4537f825f9315e141520efb4a02f422fd47f753f5ec66d9fbe.zip -phidden` (or your archive tool). You get `access.log`.
2. Find the busiest source: `awk '{print $1}' access.log | sort | uniq -c | sort -rn | head`.
3. Pull the suspect requests and their status codes: `grep -Ei 'select|union|\.\./|<script|/admin|\.php|payload' access.log` (adjust to this attack), and `awk '{print $9}' access.log | sort | uniq -c`.
4. Confirm against the answer: A POST to an upload endpoint followed by a GET of a .php in uploads

## MITRE ATT&CK
- T1505.003 Server Software Component: Web Shell

## Tools
Hand written Apache access log with real workday background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
