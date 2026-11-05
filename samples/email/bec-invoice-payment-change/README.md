# bec-invoice-payment-change

## Scenario
A business email compromise asks finance to change a supplier's bank details so future payments go to the attacker. The Reply-To uses a lookalike (capital I for l).

## Timeline
```
Supplier mail asking to change bank details for invoice 5567
```

## How to spot it
- A request to change bank/payment details
- Reply-To is a subtly different lookalike domain
- Pressure to use the new account going forward
- Finance is the target, not a link or file

## Questions
```
1. Technique?         BEC invoice fraud
2. Ask?                Change supplier bank details
3. Reply-To trick?     acme-suppIies.com (capital I)
4. Target?             finance@corp.local
5. Goal?               Redirect payments
```

## MITRE ATT&CK
- T1656 Impersonation
- T1114 Email Collection

## Tools
Hand written .eml messages with real headers; the malicious mail sits among normal business mail

## Signature
Forensic Sample Blueprint | qayssarayra.com
