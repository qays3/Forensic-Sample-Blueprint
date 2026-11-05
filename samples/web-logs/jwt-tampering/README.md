# jwt-tampering

## Scenario
An attacker tampers with a JSON Web Token (alg:none or changed claims) to escalate to admin on an API.

## Timeline (UTC+3)
```
14:35:00  GET /api/v1/admin rejected (401), then accepted (200)
14:35:xx  Forged JWT (alg none / role admin) accepted
```

## How to spot it
- Repeated calls to an admin API, first 401 then 200
- A tampered JWT (alg none or changed role claim)
- No new login between the failure and success
- Token forgery, not a stolen password

## Questions
```
1. Endpoint?          /api/v1/admin
2. Attack?            JWT tampering
3. Techniques?        alg:none, changed claims
4. Success?           401 then 200
5. Token in?          Authorization header
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 1d82df1c77187684641ed2e3f24d91ba958117862014b6cff153d99788691fde.zip -phidden` (or your archive tool). You get `access.log`.
2. Find the busiest source: `awk '{print $1}' access.log | sort | uniq -c | sort -rn | head`.
3. Pull the suspect requests and their status codes: `grep -Ei 'select|union|\.\./|<script|/admin|\.php|payload' access.log` (adjust to this attack), and `awk '{print $9}' access.log | sort | uniq -c`.
4. Confirm against the answer: Repeated calls to an admin API, first 401 then 200

## MITRE ATT&CK
- T1548 Abuse Elevation Control Mechanism

## Tools
Hand written Apache access log with real workday background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
