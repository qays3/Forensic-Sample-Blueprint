# waf-blocks-and-bypass

## Scenario
An attacker probes the WAF: the first SQLi payloads are blocked (403), then an obfuscated variant slips through (200).

## Timeline (UTC+3)
```
14:50:00  SQLi payloads blocked by WAF (403)
14:50:xx  Obfuscated payload (comments/newlines) returns 200, bypass
```

## How to spot it
- A run of 403s from the WAF on obvious payloads
- Then a 200 on an obfuscated variant (bypass)
- Comments, case, encoding used to evade
- The bypass is the dangerous line

## Questions
```
1. First responses?   403 (WAF block)
2. Bypass sign?       200 on an obfuscated payload
3. Evasion?           /**/ comments, %0a newlines
4. Base attack?       SQL injection
5. Lesson?            A block is not immunity
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 135bf70c1eeb9139922cd1d7ab690de1f6aecd08a17c982b7176f889fb58d91a.zip -phidden` (or your archive tool). You get `access.log`.
2. Find the busiest source: `awk '{print $1}' access.log | sort | uniq -c | sort -rn | head`.
3. Pull the suspect requests and their status codes: `grep -Ei 'select|union|\.\./|<script|/admin|\.php|payload' access.log` (adjust to this attack), and `awk '{print $9}' access.log | sort | uniq -c`.
4. Confirm against the answer: A run of 403s from the WAF on obvious payloads

## MITRE ATT&CK
- T1190 Exploit Public-Facing Application

## Tools
Hand written Apache access log with real workday background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
