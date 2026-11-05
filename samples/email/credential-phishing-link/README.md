# credential-phishing-link

## Scenario
A credential phishing email tells the user their password expires and links to a fake Microsoft login page. SPF, DKIM and DMARC all fail.

## Timeline
```
Phishing mail arrives claiming password expiry, linking to m365-login-verify.com
```

## How to spot it
- SPF/DKIM/DMARC failures in Authentication-Results
- Sender domain is not Microsoft
- Urgency (password expires today)
- Link to a lookalike login domain

## Questions
```
1. Lure?              Password expiry
2. Link?               m365-login-verify.com (fake)
3. Auth results?       spf fail, dkim none, dmarc fail
4. Sender domain?      m365-alerts.com
5. Goal?               Steal credentials
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x e6718549fd143b9c727682798e4dd6d75c534c158fe5671096a357cbcbdc53d4.zip -phidden` (or your archive tool). You get `inbox-00-normal.eml`.
2. Read the headers: `cat inbox-00-normal.eml`; check `Received`, `From`, `Reply-To` and `Authentication-Results` (SPF, DKIM, DMARC).
3. List the parts and decode attachments: `python3 -c "import email; m=email.message_from_file(open('inbox-00-normal.eml')); [print(p.get_content_type(), p.get_filename()) for p in m.walk()]"`.
4. Pull URLs: `grep -Eoi 'https?://[^\"> ]+' inbox-00-normal.eml`.
5. Confirm against the answer: SPF/DKIM/DMARC failures in Authentication-Results

## MITRE ATT&CK
- T1566.002 Phishing: Spearphishing Link

## Tools
Hand written .eml messages with real headers; the malicious mail sits among normal business mail

## Signature
Forensic Sample Blueprint | qayssarayra.com
