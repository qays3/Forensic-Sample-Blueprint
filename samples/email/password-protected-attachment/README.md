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

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 139185c092204e17b9c4bf9b4f44299390163f7c346d8499e8df68d22ec720d5.zip -phidden` (or your archive tool). You get `inbox-00-normal.eml`.
2. Read the headers: `cat inbox-00-normal.eml`; check `Received`, `From`, `Reply-To` and `Authentication-Results` (SPF, DKIM, DMARC).
3. List the parts and decode attachments: `python3 -c "import email; m=email.message_from_file(open('inbox-00-normal.eml')); [print(p.get_content_type(), p.get_filename()) for p in m.walk()]"`.
4. Pull URLs: `grep -Eoi 'https?://[^\"> ]+' inbox-00-normal.eml`.
5. Confirm against the answer: A password protected archive attachment

## MITRE ATT&CK
- T1566.001 Phishing: Spearphishing Attachment
- T1027 Obfuscated Files or Information

## Tools
Hand written .eml messages with real headers; the malicious mail sits among normal business mail

## Signature
Forensic Sample Blueprint | qayssarayra.com
