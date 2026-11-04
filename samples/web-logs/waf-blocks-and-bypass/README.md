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

## MITRE ATT&CK
- T1190 Exploit Public-Facing Application

## Tools
Hand written Apache access log with real workday background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
