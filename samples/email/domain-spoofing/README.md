# domain-spoofing

## Scenario
An attacker spoofs the corp.local domain in the From header, but SPF, DKIM and DMARC all fail, exposing the forgery.

## Timeline
```
Mail forged as it@corp.local but failing all authentication
```

## How to spot it
- From is the real corp.local domain
- SPF, DKIM and DMARC all fail
- Sent from an external server, not the corp mail host
- Exact domain spoof, caught by auth

## Questions
```
1. Spoofed domain?    corp.local
2. Auth results?       spf fail, dkim fail, dmarc fail
3. Real source?        45.137.22.90 (external)
4. Link?               corp-webmail-reauth.com
5. Tell?               Own domain but failing auth
```

## MITRE ATT&CK
- T1566.002 Phishing: Spearphishing Link

## Tools
Hand written .eml messages with real headers; the malicious mail sits among normal business mail

## Signature
Forensic Sample Blueprint | qayssarayra.com
