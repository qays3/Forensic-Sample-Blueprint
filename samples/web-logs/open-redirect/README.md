# open-redirect

## Scenario
An attacker abuses an open redirect, crafting links that bounce users from the trusted site to an attacker phishing page.

## Timeline (UTC+3)
```
13:05:00  /redirect?url=https://evil-login.com from 45.137.22.90
13:05:xx  302 to the attacker domain
```

## How to spot it
- A redirect parameter pointing to an external domain
- 302 responses to attacker controlled URLs
- Used to make phishing links look trusted
- // or https:// in the redirect value

## Questions
```
1. Endpoint?          /redirect (url param)
2. Attack?            Open redirect
3. Destination?       evil-login.com
4. Status?            302
5. Abuse?             Trusted link to phishing
```

## MITRE ATT&CK
- T1566.002 Phishing: Spearphishing Link

## Tools
Hand written Apache access log with real workday background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
