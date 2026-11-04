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

## MITRE ATT&CK
- T1110.004 Brute Force: Credential Stuffing

## Tools
Hand written Apache access log with real workday background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
