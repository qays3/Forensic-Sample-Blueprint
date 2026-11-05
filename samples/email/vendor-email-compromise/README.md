# vendor-email-compromise

## Scenario
A real vendor's mailbox is compromised, so authenticated mail from the genuine vendor domain carries a malicious remittance change, hard to catch.

## Timeline
```
Authenticated mail from a real but compromised vendor mailbox
```

## How to spot it
- Mail from a genuine, authenticated vendor domain
- A remittance or payment change request
- Comes from a real prior contact (compromised account)
- Authentication passes, so content and context matter

## Questions
```
1. Technique?         Vendor email compromise
2. Sender?             Real vendor (compromised)
3. Auth?               Passes
4. Ask?                Remittance update
5. Why hard?           Genuine domain and contact
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 0ef33cb4e82eb7bea8d5a8298d40f1967d6e357d6412ce10be7ccd89b910cafb.zip -phidden` (or your archive tool). You get `inbox-00-normal.eml`.
2. Read the headers: `cat inbox-00-normal.eml`; check `Received`, `From`, `Reply-To` and `Authentication-Results` (SPF, DKIM, DMARC).
3. List the parts and decode attachments: `python3 -c "import email; m=email.message_from_file(open('inbox-00-normal.eml')); [print(p.get_content_type(), p.get_filename()) for p in m.walk()]"`.
4. Pull URLs: `grep -Eoi 'https?://[^\"> ]+' inbox-00-normal.eml`.
5. Confirm against the answer: Mail from a genuine, authenticated vendor domain

## MITRE ATT&CK
- T1566 Phishing
- T1114 Email Collection

## Tools
Hand written .eml messages with real headers; the malicious mail sits among normal business mail

## Signature
Forensic Sample Blueprint | qayssarayra.com
