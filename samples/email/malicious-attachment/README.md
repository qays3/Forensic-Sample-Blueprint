# malicious-attachment

## Scenario
A macro enabled Word document (.docm) is attached to a fake overdue invoice email to lure the user into enabling macros.

## Timeline
```
Fake overdue invoice with an Invoice_88213.docm macro attachment
```

## How to spot it
- A macro enabled attachment (.docm)
- Invoice lure with urgency
- Sender domain fails authentication
- Attachment type that runs code on enable

## Questions
```
1. Attachment?        Invoice_88213.docm (macro)
2. Lure?               Overdue invoice
3. Auth?               spf/dmarc fail
4. Sender?             supplier-invoices.com
5. Goal?               Macro execution
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 2b99834ce9f379a12f2f29b3cf4301c49c72b75c8a209d7087fb102c867c05e9.zip -phidden` (or your archive tool). You get `inbox-00-normal.eml`.
2. Read the headers: `cat inbox-00-normal.eml`; check `Received`, `From`, `Reply-To` and `Authentication-Results` (SPF, DKIM, DMARC).
3. List the parts and decode attachments: `python3 -c "import email; m=email.message_from_file(open('inbox-00-normal.eml')); [print(p.get_content_type(), p.get_filename()) for p in m.walk()]"`.
4. Pull URLs: `grep -Eoi 'https?://[^\"> ]+' inbox-00-normal.eml`.
5. Confirm against the answer: A macro enabled attachment (.docm)

## MITRE ATT&CK
- T1566.001 Phishing: Spearphishing Attachment

## Tools
Hand written .eml messages with real headers; the malicious mail sits among normal business mail

## Signature
Forensic Sample Blueprint | qayssarayra.com
