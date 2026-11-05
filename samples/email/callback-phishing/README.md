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

## MITRE ATT&CK
- T1566 Phishing
- T1598 Phishing for Information

## Tools
Hand written .eml messages with real headers; the malicious mail sits among normal business mail

## Signature
Forensic Sample Blueprint | qayssarayra.com
