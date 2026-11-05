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

## MITRE ATT&CK
- T1566.001 Phishing: Spearphishing Attachment

## Tools
Hand written .eml messages with real headers; the malicious mail sits among normal business mail

## Signature
Forensic Sample Blueprint | qayssarayra.com
