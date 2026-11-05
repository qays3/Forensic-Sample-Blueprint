# cloud-hosted-phishing-link

## Scenario
The phishing page is hosted on a real cloud service (SharePoint/OneDrive), so the link is to a trusted domain and the mail authenticates, with the fake login one click deeper.

## Timeline
```
Mail linking to a phishing page hosted on real SharePoint
```

## How to spot it
- A link to a legitimate cloud domain (SharePoint)
- The malicious login is a click deeper, on the hosted page
- Mail authenticates, link looks safe
- Payroll lure

## Questions
```
1. Technique?         Cloud hosted phishing
2. Host?               real SharePoint domain
3. Lure?               Payroll_2026.xlsx
4. Why hard?           Trusted domain + passing auth
5. Payload?            Fake login on the page
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 8ea12de94804ee18ac90a049e1f8358b2b59a3d01e5ecc43e9e9e96d2a501bc0.zip -phidden` (or your archive tool). You get `inbox-00-normal.eml`.
2. Read the headers: `cat inbox-00-normal.eml`; check `Received`, `From`, `Reply-To` and `Authentication-Results` (SPF, DKIM, DMARC).
3. List the parts and decode attachments: `python3 -c "import email; m=email.message_from_file(open('inbox-00-normal.eml')); [print(p.get_content_type(), p.get_filename()) for p in m.walk()]"`.
4. Pull URLs: `grep -Eoi 'https?://[^\"> ]+' inbox-00-normal.eml`.
5. Confirm against the answer: A link to a legitimate cloud domain (SharePoint)

## MITRE ATT&CK
- T1566.002 Phishing: Spearphishing Link

## Tools
Hand written .eml messages with real headers; the malicious mail sits among normal business mail

## Signature
Forensic Sample Blueprint | qayssarayra.com
