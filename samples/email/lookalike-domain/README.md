# lookalike-domain

## Scenario
A lookalike domain (corp-finance.co instead of corp.local) passes authentication because the attacker owns it, so only the domain itself gives it away.

## Timeline
```
Mail from corp-finance.co, a lookalike of the real corp domain
```

## How to spot it
- Domain is a near match of the real one (corp-finance.co)
- Authentication passes (attacker owns the domain)
- Only careful reading of the domain reveals it
- Finance/payment theme

## Questions
```
1. Lookalike?         corp-finance.co
2. Real domain?        corp.local
3. Auth?               Passes (attacker domain)
4. Tell?               The domain itself
5. Theme?              Payment
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 26041b6bf2bd6a2d2b5697154b12b28cf88e4e34a11b84bb386acfee425b1932.zip -phidden` (or your archive tool). You get `inbox-00-normal.eml`.
2. Read the headers: `cat inbox-00-normal.eml`; check `Received`, `From`, `Reply-To` and `Authentication-Results` (SPF, DKIM, DMARC).
3. List the parts and decode attachments: `python3 -c "import email; m=email.message_from_file(open('inbox-00-normal.eml')); [print(p.get_content_type(), p.get_filename()) for p in m.walk()]"`.
4. Pull URLs: `grep -Eoi 'https?://[^\"> ]+' inbox-00-normal.eml`.
5. Confirm against the answer: Domain is a near match of the real one (corp-finance.co)

## MITRE ATT&CK
- T1566 Phishing
- T1583.001 Acquire Infrastructure: Domains

## Tools
Hand written .eml messages with real headers; the malicious mail sits among normal business mail

## Signature
Forensic Sample Blueprint | qayssarayra.com
