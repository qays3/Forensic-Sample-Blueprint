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

## MITRE ATT&CK
- T1595 Active Scanning
- T1552.001 Credentials In Files

## Tools
Hand written Apache access log with real workday background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
