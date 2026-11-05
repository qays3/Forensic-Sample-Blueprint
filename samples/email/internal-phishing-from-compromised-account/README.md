# internal-phishing-from-compromised-account

## Scenario
A compromised internal mailbox (sara) sends phishing to colleagues; it authenticates and comes from a trusted coworker, so it is very convincing.

## Timeline
```
Phishing from a real internal account (sara) to colleagues
```

## How to spot it
- Mail from a genuine internal, authenticated account
- Sent to many colleagues
- A link to an external lookalike portal
- Trust of an internal sender

## Questions
```
1. Sender?            sara@corp.local (compromised)
2. Auth?               Passes (really internal)
3. Link?               corp-portal-sign.com
4. Targets?            Colleagues
5. Why effective?      Trusted internal sender
```

## MITRE ATT&CK
- T1534 Internal Spearphishing

## Tools
Hand written .eml messages with real headers; the malicious mail sits among normal business mail

## Signature
Forensic Sample Blueprint | qayssarayra.com
