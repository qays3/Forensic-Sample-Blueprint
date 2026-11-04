# login-brute-force

## Scenario
An attacker brute forces the login form for one account, sending many failed POSTs until one succeeds.

## Timeline (UTC+3)
```
11:25:00  Many failed POST /login (401) from 45.137.22.90
11:2x:xx  One POST /login returns 302, login success
```

## How to spot it
- Many POSTs to /login with 401 from one IP
- A sudden 302/200 after the failures
- One account, many password tries
- High rate, tool like

## Questions
```
1. Attack?            Login brute force
2. Endpoint?          /login
3. Fail status?       401
4. Success?           302 after ~120 tries
5. Attacker?          45.137.22.90
```

## MITRE ATT&CK
- T1110.001 Brute Force: Password Guessing

## Tools
Hand written Apache access log with real workday background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
