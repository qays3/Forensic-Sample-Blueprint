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

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 7671597e96dda149c0ada42bc24f6527b49b19987a443f6dba653fec94ec9f67.zip -phidden` (or your archive tool). You get `access.log`.
2. Find the busiest source: `awk '{print $1}' access.log | sort | uniq -c | sort -rn | head`.
3. Pull the suspect requests and their status codes: `grep -Ei 'select|union|\.\./|<script|/admin|\.php|payload' access.log` (adjust to this attack), and `awk '{print $9}' access.log | sort | uniq -c`.
4. Confirm against the answer: A redirect parameter pointing to an external domain

## MITRE ATT&CK
- T1566.002 Phishing: Spearphishing Link

## Tools
Hand written Apache access log with real workday background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
