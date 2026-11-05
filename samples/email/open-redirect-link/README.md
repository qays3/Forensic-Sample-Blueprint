# open-redirect-link

## Scenario
The phishing link uses a trusted domain's open redirect (google.com/url?q=) to bounce the victim to the attacker site, so the visible domain looks safe.

## Timeline
```
Mail with a google.com/url open redirect to an attacker page
```

## How to spot it
- A link whose host is a trusted domain
- A redirect parameter pointing elsewhere (q=evil-login.com)
- The trusted domain hides the real destination
- Passes authentication (really from Google share)

## Questions
```
1. Technique?         Open redirect abuse
2. Visible host?       www.google.com
3. Real destination?   evil-login.com
4. Parameter?          url?q=
5. Why effective?      Trusted domain in the link
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 95deffd9c214e0c209cd487c627e826eb54ce39cc828992433dec0aefe70007f.zip -phidden` (or your archive tool). You get `inbox-00-normal.eml`.
2. Read the headers: `cat inbox-00-normal.eml`; check `Received`, `From`, `Reply-To` and `Authentication-Results` (SPF, DKIM, DMARC).
3. List the parts and decode attachments: `python3 -c "import email; m=email.message_from_file(open('inbox-00-normal.eml')); [print(p.get_content_type(), p.get_filename()) for p in m.walk()]"`.
4. Pull URLs: `grep -Eoi 'https?://[^\"> ]+' inbox-00-normal.eml`.
5. Confirm against the answer: A link whose host is a trusted domain

## MITRE ATT&CK
- T1566.002 Phishing: Spearphishing Link

## Tools
Hand written .eml messages with real headers; the malicious mail sits among normal business mail

## Signature
Forensic Sample Blueprint | qayssarayra.com
