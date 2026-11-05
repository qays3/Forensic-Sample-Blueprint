# reply-to-mismatch

## Scenario
The From looks internal and authenticates, but the Reply-To points to an external address, so any reply (and the payment thread) goes to the attacker.

## Timeline
```
Internal looking approval mail with an external Reply-To
```

## How to spot it
- From is internal and passes auth
- Reply-To is a different external domain
- Replies leave the organization to the attacker
- Subtle header mismatch

## Questions
```
1. From?              procurement@corp.local (internal)
2. Reply-To?           collector@external-billing.net
3. Mismatch?           From vs Reply-To
4. Effect?             Replies go to attacker
5. Theme?              Payment approval
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x ab3f1f89384a9d1f8073b4d63051f315b84f8f341f2dc56aee8a167d3c727386.zip -phidden` (or your archive tool). You get `inbox-00-normal.eml`.
2. Read the headers: `cat inbox-00-normal.eml`; check `Received`, `From`, `Reply-To` and `Authentication-Results` (SPF, DKIM, DMARC).
3. List the parts and decode attachments: `python3 -c "import email; m=email.message_from_file(open('inbox-00-normal.eml')); [print(p.get_content_type(), p.get_filename()) for p in m.walk()]"`.
4. Pull URLs: `grep -Eoi 'https?://[^\"> ]+' inbox-00-normal.eml`.
5. Confirm against the answer: From is internal and passes auth

## MITRE ATT&CK
- T1566 Phishing
- T1114 Email Collection

## Tools
Hand written .eml messages with real headers; the malicious mail sits among normal business mail

## Signature
Forensic Sample Blueprint | qayssarayra.com
