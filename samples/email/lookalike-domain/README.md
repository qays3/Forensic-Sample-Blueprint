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

## MITRE ATT&CK
- T1566 Phishing
- T1583.001 Acquire Infrastructure: Domains

## Tools
Hand written .eml messages with real headers; the malicious mail sits among normal business mail

## Signature
Forensic Sample Blueprint | qayssarayra.com
