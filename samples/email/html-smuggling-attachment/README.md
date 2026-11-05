# html-smuggling-attachment

## Scenario
An HTML attachment uses HTML smuggling to assemble a malicious file in the browser, bypassing mail gateway scanning.

## Timeline
```
DocuSign themed mail with a Document.html smuggling attachment
```

## How to spot it
- An .html attachment, not a real document
- HTML smuggling builds the payload client side
- Mail gateway sees only benign HTML
- Lookalike DocuSign sender

## Questions
```
1. Attachment?        Document.html
2. Technique?          HTML smuggling
3. Why it evades?      Payload assembled in browser
4. Theme?              DocuSign
5. Sender?             docu-sign-secure.com
```

## MITRE ATT&CK
- T1566.001 Phishing: Spearphishing Attachment
- T1027.006 HTML Smuggling

## Tools
Hand written .eml messages with real headers; the malicious mail sits among normal business mail

## Signature
Forensic Sample Blueprint | qayssarayra.com
