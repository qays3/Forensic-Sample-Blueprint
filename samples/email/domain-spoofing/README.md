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

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 6b213741ddfaaa9887e357a832864cfe001cd00e5c60715cd1a99d7fd83e7715.zip -phidden` (or your archive tool). You get `inbox-00-normal.eml`.
2. Read the headers: `cat inbox-00-normal.eml`; check `Received`, `From`, `Reply-To` and `Authentication-Results` (SPF, DKIM, DMARC).
3. List the parts and decode attachments: `python3 -c "import email; m=email.message_from_file(open('inbox-00-normal.eml')); [print(p.get_content_type(), p.get_filename()) for p in m.walk()]"`.
4. Pull URLs: `grep -Eoi 'https?://[^\"> ]+' inbox-00-normal.eml`.
5. Confirm against the answer: From is the real corp.local domain

## MITRE ATT&CK
- T1566.002 Phishing: Spearphishing Link

## Tools
Hand written .eml messages with real headers; the malicious mail sits among normal business mail

## Signature
Forensic Sample Blueprint | qayssarayra.com
