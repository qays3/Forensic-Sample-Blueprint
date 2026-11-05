# password-reset-abuse

## Scenario
An attacker floods the password reset endpoint and then brute forces or reuses reset tokens to take over an account.

## Timeline (UTC+3)
```
13:30:00  Many POST /reset then token guessing on /reset?token=
13:30:xx  Sequential/again token values tried
```

## How to spot it
- A burst of password reset requests
- Then many /reset?token= attempts
- Token values that look guessed or reused
- Account takeover via reset flow

## Questions
```
1. Endpoint?          /reset
2. Attack?            Password reset abuse
3. Steps?             Flood resets, then guess tokens
4. Goal?              Account takeover
5. Token param?       ?token=
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 0e6469cb6c52086ace4b7c3b5be4b560ce34cc146e7e64c31d464cfe61d93050.zip -phidden` (or your archive tool). You get `access.log`.
2. Find the busiest source: `awk '{print $1}' access.log | sort | uniq -c | sort -rn | head`.
3. Pull the suspect requests and their status codes: `grep -Ei 'select|union|\.\./|<script|/admin|\.php|payload' access.log` (adjust to this attack), and `awk '{print $9}' access.log | sort | uniq -c`.
4. Confirm against the answer: A burst of password reset requests

## MITRE ATT&CK
- T1098 Account Manipulation

## Tools
Hand written Apache access log with real workday background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
