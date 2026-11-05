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

## MITRE ATT&CK
- T1566.002 Phishing: Spearphishing Link

## Tools
Hand written .eml messages with real headers; the malicious mail sits among normal business mail

## Signature
Forensic Sample Blueprint | qayssarayra.com
