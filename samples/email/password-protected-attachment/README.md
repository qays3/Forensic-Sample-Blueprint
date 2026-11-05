# password-protected-attachment

## Scenario
A password protected zip hides the malicious payload from the mail gateway, with the password helpfully included in the email body.

## Timeline
```
Password protected Payslip.zip with the password in the body
```

## How to spot it
- A password protected archive attachment
- The password given in the email text
- Encryption blinds the gateway scanner
- Payslip lure

## Questions
```
1. Attachment?        Payslip.zip (password protected)
2. Password?           In the body (1234)
3. Why protected?      Evade gateway scanning
4. Auth?               spf/dmarc fail
5. Lure?               Payslip
```

## MITRE ATT&CK
- T1566.001 Phishing: Spearphishing Attachment
- T1027 Obfuscated Files or Information

## Tools
Hand written .eml messages with real headers; the malicious mail sits among normal business mail

## Signature
Forensic Sample Blueprint | qayssarayra.com
