# api-enumeration

## Scenario
An attacker enumerates API endpoints, probing for hidden or unauthorized routes and reading responses by status code.

## Timeline (UTC+3)
```
14:40:00  Rapid probing of /api/v1/* paths from 45.137.22.90
14:40:xx  Mix of 200/401/403/404 maps the API
```

## How to spot it
- Rapid requests across many API paths
- Status codes used to map what exists
- Probes for admin, config, keys, debug, swagger
- One IP, tool like rate

## Questions
```
1. Attack?            API enumeration
2. Base path?         /api/v1/
3. Sensitive probes?  admin, config, keys, debug
4. Method?            Status code mapping
5. Doc leak?          swagger.json
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 37976af022e3896d115409991757505c2b7ead1c52ae1d84f68d0e3360a0b0d5.zip -phidden` (or your archive tool). You get `access.log`.
2. Find the busiest source: `awk '{print $1}' access.log | sort | uniq -c | sort -rn | head`.
3. Pull the suspect requests and their status codes: `grep -Ei 'select|union|\.\./|<script|/admin|\.php|payload' access.log` (adjust to this attack), and `awk '{print $9}' access.log | sort | uniq -c`.
4. Confirm against the answer: Rapid requests across many API paths

## MITRE ATT&CK
- T1595 Active Scanning

## Tools
Hand written Apache access log with real workday background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
