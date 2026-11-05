# callback-phishing

## Scenario
A callback phishing (TOA) email has no link or attachment, only a phone number, luring the victim to call a fake support line that walks them into installing remote access.

## Timeline
```
Fake renewal charge mail with only a phone number to call
```

## How to spot it
- No link or attachment, just a phone number
- A surprise charge to prompt a call
- The attack continues over the phone
- Email scanners find nothing malicious

## Questions
```
1. Technique?         Callback phishing (TOAD)
2. Payload?            Phone number only
3. Lure?               Unexpected $499.99 charge
4. Why no link?        Move to phone, evade scanning
5. Next step?          Fake support installs RAT
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x ea6ec3b5f5549c346d7cbce8f04b65902cc8406caf88fcbb668ca1820f996489.zip -phidden` (or your archive tool). You get `inbox-00-normal.eml`.
2. Read the headers: `cat inbox-00-normal.eml`; check `Received`, `From`, `Reply-To` and `Authentication-Results` (SPF, DKIM, DMARC).
3. List the parts and decode attachments: `python3 -c "import email; m=email.message_from_file(open('inbox-00-normal.eml')); [print(p.get_content_type(), p.get_filename()) for p in m.walk()]"`.
4. Pull URLs: `grep -Eoi 'https?://[^\"> ]+' inbox-00-normal.eml`.
5. Confirm against the answer: No link or attachment, just a phone number

## MITRE ATT&CK
- T1566 Phishing
- T1598 Phishing for Information

## Tools
Hand written .eml messages with real headers; the malicious mail sits among normal business mail

## Signature
Forensic Sample Blueprint | qayssarayra.com
