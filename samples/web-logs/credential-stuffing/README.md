# credential-stuffing

## Scenario
An attacker replays leaked username and password pairs against the login form (credential stuffing), one try per account, with a few hits.

## Timeline (UTC+3)
```
10:05:00  Many POST /login from 185.220.101.7, one try each
10:0x:xx  Mostly 401, a couple of 302 hits
```

## How to spot it
- Many logins, each a different username, one attempt each
- Distributed try pattern, not one account
- A few succeed (reused leaked passwords)
- Often from hosting or proxy IPs

## Questions
```
1. Attack?            Credential stuffing
2. Pattern?           Many users, one try each
3. Fail/success?      401 mostly, a few 302
4. Source?            185.220.101.7
5. Root cause?        Reused leaked passwords
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x de1b1a7e79d972df03824ef78d60449e1950f225dc350fb94595c2b6e801570d.zip -phidden` (or your archive tool). You get `access.log`.
2. Find the busiest source: `awk '{print $1}' access.log | sort | uniq -c | sort -rn | head`.
3. Pull the suspect requests and their status codes: `grep -Ei 'select|union|\.\./|<script|/admin|\.php|payload' access.log` (adjust to this attack), and `awk '{print $9}' access.log | sort | uniq -c`.
4. Confirm against the answer: Many logins, each a different username, one attempt each

## MITRE ATT&CK
- T1110.004 Brute Force: Credential Stuffing

## Tools
Hand written Apache access log with real workday background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
