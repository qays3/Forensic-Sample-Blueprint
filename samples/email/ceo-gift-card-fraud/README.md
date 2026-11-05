# ceo-gift-card-fraud

## Scenario
A CEO fraud email impersonates the executive and asks the user to buy gift cards and send the codes, with secrecy and urgency.

## Timeline
```
Mail posing as the CEO asking to buy gift cards and send codes
```

## How to spot it
- Executive impersonation from a free mail account
- Request for gift cards and codes
- Urgency plus a secrecy ask
- No link or attachment, pure social engineering

## Questions
```
1. Technique?         CEO gift card fraud
2. Ask?                Buy gift cards, send codes
3. Red flags?          Secrecy, urgency, free mail
4. Impersonated?       The CEO
5. Goal?               Direct theft
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 734026b3aabf821f7defd7d8df71a4bc42c0fc113c8b02cb8b051de532d8e0f5.zip -phidden` (or your archive tool). You get `inbox-00-normal.eml`.
2. Read the headers: `cat inbox-00-normal.eml`; check `Received`, `From`, `Reply-To` and `Authentication-Results` (SPF, DKIM, DMARC).
3. List the parts and decode attachments: `python3 -c "import email; m=email.message_from_file(open('inbox-00-normal.eml')); [print(p.get_content_type(), p.get_filename()) for p in m.walk()]"`.
4. Pull URLs: `grep -Eoi 'https?://[^\"> ]+' inbox-00-normal.eml`.
5. Confirm against the answer: Executive impersonation from a free mail account

## MITRE ATT&CK
- T1656 Impersonation

## Tools
Hand written .eml messages with real headers; the malicious mail sits among normal business mail

## Signature
Forensic Sample Blueprint | qayssarayra.com
