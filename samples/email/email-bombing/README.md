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

## MITRE ATT&CK
- T1498 Network Denial of Service
- T1656 Impersonation

## Tools
Hand written .eml messages with real headers; the malicious mail sits among normal business mail

## Signature
Forensic Sample Blueprint | qayssarayra.com
