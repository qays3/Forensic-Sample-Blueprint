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

## MITRE ATT&CK
- T1566 Phishing
- T1114 Email Collection

## Tools
Hand written .eml messages with real headers; the malicious mail sits among normal business mail

## Signature
Forensic Sample Blueprint | qayssarayra.com
