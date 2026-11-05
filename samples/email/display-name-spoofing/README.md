# display-name-spoofing

## Scenario
The display name shows a real manager (Khalid Mansour) but the actual address is a random Gmail, a display name spoof opening a BEC approach.

## Timeline
```
Mail that looks like it is from the manager but is a random Gmail address
```

## How to spot it
- Display name is a known executive, address is a free mail account
- Authentication passes (it really is Gmail)
- Vague urgent request to open a dialogue
- No link or attachment yet, social engineering

## Questions
```
1. Shown name?        Khalid Mansour (manager)
2. Real address?       random5521@gmail.com
3. Auth?               Passes (genuine Gmail)
4. Technique?          Display name spoofing
5. Goal?               Start a BEC
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 2b174c931cc437f5a2871fe13b19cb82a21c634e3a34aa53b38587026fe47c2d.zip -phidden` (or your archive tool). You get `inbox-00-normal.eml`.
2. Read the headers: `cat inbox-00-normal.eml`; check `Received`, `From`, `Reply-To` and `Authentication-Results` (SPF, DKIM, DMARC).
3. List the parts and decode attachments: `python3 -c "import email; m=email.message_from_file(open('inbox-00-normal.eml')); [print(p.get_content_type(), p.get_filename()) for p in m.walk()]"`.
4. Pull URLs: `grep -Eoi 'https?://[^\"> ]+' inbox-00-normal.eml`.
5. Confirm against the answer: Display name is a known executive, address is a free mail account

## MITRE ATT&CK
- T1534 Internal Spearphishing
- T1656 Impersonation

## Tools
Hand written .eml messages with real headers; the malicious mail sits among normal business mail

## Signature
Forensic Sample Blueprint | qayssarayra.com
