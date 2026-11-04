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

## MITRE ATT&CK
- T1595 Active Scanning

## Tools
Hand written Apache access log with real workday background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
