# directory-brute-force

## Scenario
An external host (45.137.22.90) runs a directory and file brute force against intranet.corp.local, requesting hundreds of common paths in seconds. Most return 404, a few 403 or 200.

## Timeline (UTC+3)
```
11:00:00  Burst of requests to common paths from 45.137.22.90
11:0x:xx  ~60 requests, mostly 404, a few 403/200
```

## How to spot it
- A burst of requests from one IP in seconds
- Mostly 404 with occasional 403/200
- Requests for admin, backup, .git, phpmyadmin, shell.php
- A tool user agent (curl) or none

## Questions
```
1. Attacker IP?        45.137.22.90
2. Pattern?            Directory/file brute force
3. Main status code?   404
4. Interesting hits?   200/403 paths
5. Tool sign?          High rate, curl UA
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 55dba141b469dca76fd4a7ec1b84e4e6d8b08758e8ce9b1d11f38b1c906b1ff6.zip -phidden` (or your archive tool). You get `access.log`.
2. Find the busiest source: `awk '{print $1}' access.log | sort | uniq -c | sort -rn | head`.
3. Pull the suspect requests and their status codes: `grep -Ei 'select|union|\.\./|<script|/admin|\.php|payload' access.log` (adjust to this attack), and `awk '{print $9}' access.log | sort | uniq -c`.
4. Confirm against the answer: A burst of requests from one IP in seconds

## MITRE ATT&CK
- T1595.003 Active Scanning: Wordlist Scanning

## Tools
Hand written Apache access log with real workday background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
