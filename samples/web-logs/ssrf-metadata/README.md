# ssrf-metadata

## Scenario
An attacker abuses a URL fetch feature (SSRF) to reach the cloud metadata service at 169.254.169.254 and steal instance credentials.

## Timeline (UTC+3)
```
14:25:00  SSRF on /fetch?url= pointing at 169.254.169.254
14:25:xx  Reads IAM security-credentials from metadata
```

## How to spot it
- A url parameter pointed at 169.254.169.254 or localhost
- Server side fetch of the cloud metadata endpoint
- Reads of iam/security-credentials
- Internal targets a user could not reach directly

## Questions
```
1. Endpoint?          /fetch (url param)
2. Attack?            SSRF
3. Target?            169.254.169.254 metadata
4. Stolen?            IAM credentials
5. Also probed?       localhost:8080/admin
```

## MITRE ATT&CK
- T1190 Exploit Public-Facing Application
- T1552.005 Cloud Instance Metadata API

## Tools
Hand written Apache access log with real workday background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
