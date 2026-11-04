# nosql-injection

## Scenario
An attacker sends NoSQL injection in a JSON login body to /api/v1/login, using operators like $ne and $gt to bypass authentication. The third attempt returns 200 (bypassed).

## Timeline (UTC+3)
```
13:10:00  POST /api/v1/login with $ne/$gt operators from 45.137.22.90
13:10:xx  Third attempt returns 200, auth bypassed
```

## How to spot it
- POST to a login API with JSON operator payloads ($ne, $gt)
- Repeated auth attempts, then a sudden 200
- No password actually known, logic is bypassed
- Body carries the operators (seen in app logs or a proxy)

## Questions
```
1. Endpoint?          /api/v1/login
2. Injection type?    NoSQL ($ne, $gt operators)
3. Goal?              Authentication bypass
4. Success status?    200
5. Note?              Payload is in the JSON body
```

## MITRE ATT&CK
- T1190 Exploit Public-Facing Application

## Tools
Hand written Apache access log; the JSON operator payload lives in the request body (app log)

## Signature
Forensic Sample Blueprint | qayssarayra.com
