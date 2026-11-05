# exposed-git-env

## Scenario
An attacker requests exposed /.git and /.env files, which leak source code and secrets when accidentally deployed.

## Timeline (UTC+3)
```
11:35:00  GET /.git/config, /.git/HEAD, /.env from 45.137.22.90
11:35:xx  200 responses leak repo and secrets
```

## How to spot it
- Requests for /.git/ internals and /.env
- 200 (not 404) means the files are exposed
- Leaks source history and environment secrets
- Common misconfiguration hunt

## Questions
```
1. Files targeted?    /.git/config, /.env
2. Risk?              Source code and secrets leak
3. Exposed sign?      HTTP 200 on those paths
4. Attacker?          45.137.22.90
5. Cause?             Deploying the .git/.env
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 3e806dc7ef337674342ad8c204bc82c4e81324f86ffc7aa9c19da47728a5c405.zip -phidden` (or your archive tool). You get `access.log`.
2. Find the busiest source: `awk '{print $1}' access.log | sort | uniq -c | sort -rn | head`.
3. Pull the suspect requests and their status codes: `grep -Ei 'select|union|\.\./|<script|/admin|\.php|payload' access.log` (adjust to this attack), and `awk '{print $9}' access.log | sort | uniq -c`.
4. Confirm against the answer: Requests for /.git/ internals and /.env

## MITRE ATT&CK
- T1595 Active Scanning
- T1552.001 Credentials In Files

## Tools
Hand written Apache access log with real workday background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
