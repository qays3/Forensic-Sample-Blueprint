# internal-phishing-from-compromised-account

## Scenario
A compromised internal mailbox (sara) sends phishing to colleagues; it authenticates and comes from a trusted coworker, so it is very convincing.

## Timeline
```
Phishing from a real internal account (sara) to colleagues
```

## How to spot it
- Mail from a genuine internal, authenticated account
- Sent to many colleagues
- A link to an external lookalike portal
- Trust of an internal sender

## Questions
```
1. Sender?            sara@corp.local (compromised)
2. Auth?               Passes (really internal)
3. Link?               corp-portal-sign.com
4. Targets?            Colleagues
5. Why effective?      Trusted internal sender
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 352966cccf1dae1aff4aace67e1227239d828850d9b4fb794798a48c7093e305.zip -phidden` (or your archive tool). You get `inbox-00-normal.eml`.
2. Read the headers: `cat inbox-00-normal.eml`; check `Received`, `From`, `Reply-To` and `Authentication-Results` (SPF, DKIM, DMARC).
3. List the parts and decode attachments: `python3 -c "import email; m=email.message_from_file(open('inbox-00-normal.eml')); [print(p.get_content_type(), p.get_filename()) for p in m.walk()]"`.
4. Pull URLs: `grep -Eoi 'https?://[^\"> ]+' inbox-00-normal.eml`.
5. Confirm against the answer: Mail from a genuine internal, authenticated account

## MITRE ATT&CK
- T1534 Internal Spearphishing

## Tools
Hand written .eml messages with real headers; the malicious mail sits among normal business mail

## Signature
Forensic Sample Blueprint | qayssarayra.com
