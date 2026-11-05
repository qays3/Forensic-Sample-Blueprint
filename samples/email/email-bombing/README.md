# email-bombing

## Scenario
An email bomb floods the victim with thousands of subscription confirmations to bury a real alert (often a fraud confirmation) in the noise.

## Timeline
```
A flood of subscription confirmation emails in a short time
```

## How to spot it
- Hundreds of signup/confirmation mails in minutes
- Each from a different legitimate service
- Meant to bury an important alert
- Inbox flooded as a smokescreen

## Questions
```
1. Technique?         Email bombing
2. Volume?             Thousands of confirmations
3. Goal?               Bury a real alert (e.g. fraud)
4. Each mail?          A real signup confirmation
5. Effect?             Inbox flooded
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x e87404a949e27ea06568528d5a24cf9bc2a5a9720d435a8327c2008e4f3ec51b.zip -phidden` (or your archive tool). You get `inbox-00-normal.eml`.
2. Read the headers: `cat inbox-00-normal.eml`; check `Received`, `From`, `Reply-To` and `Authentication-Results` (SPF, DKIM, DMARC).
3. List the parts and decode attachments: `python3 -c "import email; m=email.message_from_file(open('inbox-00-normal.eml')); [print(p.get_content_type(), p.get_filename()) for p in m.walk()]"`.
4. Pull URLs: `grep -Eoi 'https?://[^\"> ]+' inbox-00-normal.eml`.
5. Confirm against the answer: Hundreds of signup/confirmation mails in minutes

## MITRE ATT&CK
- T1498 Network Denial of Service
- T1656 Impersonation

## Tools
Hand written .eml messages with real headers; the malicious mail sits among normal business mail

## Signature
Forensic Sample Blueprint | qayssarayra.com
