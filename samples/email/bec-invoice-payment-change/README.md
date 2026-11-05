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

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 8b4a624d17941304c98318828a0ff7fdd68c83535a21c16228b63b70212d6f15.zip -phidden` (or your archive tool). You get `inbox-00-normal.eml`.
2. Read the headers: `cat inbox-00-normal.eml`; check `Received`, `From`, `Reply-To` and `Authentication-Results` (SPF, DKIM, DMARC).
3. List the parts and decode attachments: `python3 -c "import email; m=email.message_from_file(open('inbox-00-normal.eml')); [print(p.get_content_type(), p.get_filename()) for p in m.walk()]"`.
4. Pull URLs: `grep -Eoi 'https?://[^\"> ]+' inbox-00-normal.eml`.
5. Confirm against the answer: A request to change bank/payment details

## MITRE ATT&CK
- T1656 Impersonation
- T1114 Email Collection

## Tools
Hand written .eml messages with real headers; the malicious mail sits among normal business mail

## Signature
Forensic Sample Blueprint | qayssarayra.com
