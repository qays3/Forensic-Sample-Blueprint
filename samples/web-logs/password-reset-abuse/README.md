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

## MITRE ATT&CK
- T1098 Account Manipulation

## Tools
Hand written Apache access log with real workday background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
