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

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 6bad6d0213a8a74e02cd5d58d53cacd72de80cf2160790de5682859c42856dfa.zip -phidden` (or your archive tool). You get `access.log`.
2. Find the busiest source: `awk '{print $1}' access.log | sort | uniq -c | sort -rn | head`.
3. Pull the suspect requests and their status codes: `grep -Ei 'select|union|\.\./|<script|/admin|\.php|payload' access.log` (adjust to this attack), and `awk '{print $9}' access.log | sort | uniq -c`.
4. Confirm against the answer: Many POSTs to /login with 401 from one IP

## MITRE ATT&CK
- T1110.001 Brute Force: Password Guessing

## Tools
Hand written Apache access log with real workday background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
