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

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 4d666b3246189fe63d9dfea4806dc12148febc8349308934293c249c6f47b078.zip -phidden` (or your archive tool). You get `inbox-00-normal.eml`.
2. Read the headers: `cat inbox-00-normal.eml`; check `Received`, `From`, `Reply-To` and `Authentication-Results` (SPF, DKIM, DMARC).
3. List the parts and decode attachments: `python3 -c "import email; m=email.message_from_file(open('inbox-00-normal.eml')); [print(p.get_content_type(), p.get_filename()) for p in m.walk()]"`.
4. Pull URLs: `grep -Eoi 'https?://[^\"> ]+' inbox-00-normal.eml`.
5. Confirm against the answer: An .html attachment, not a real document

## MITRE ATT&CK
- T1566.001 Phishing: Spearphishing Attachment
- T1027.006 HTML Smuggling

## Tools
Hand written .eml messages with real headers; the malicious mail sits among normal business mail

## Signature
Forensic Sample Blueprint | qayssarayra.com
