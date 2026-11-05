# qr-code-phishing

## Scenario
A quishing (QR phishing) email embeds a QR code pointing to a credential harvesting site, moving the attack to the user's phone to dodge email link scanning.

## Timeline
```
MFA themed mail with a QR code to scan
```

## How to spot it
- The actionable link is inside a QR image
- Little or no clickable text link
- Moves the victim to a phone, off the managed device
- MFA re-enrollment lure

## Questions
```
1. Technique?         QR code phishing (quishing)
2. Payload?            QR image (qr.png)
3. Lure?               MFA re-enrollment
4. Why QR?             Evade link scanning, pivot to phone
5. Sender?             corp-secure-portal.com
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 02af75f70783cbbe1b462f3f3cc290ce8c3519ca33b015b38c744f273ca6af76.zip -phidden` (or your archive tool). You get `inbox-00-normal.eml`.
2. Read the headers: `cat inbox-00-normal.eml`; check `Received`, `From`, `Reply-To` and `Authentication-Results` (SPF, DKIM, DMARC).
3. List the parts and decode attachments: `python3 -c "import email; m=email.message_from_file(open('inbox-00-normal.eml')); [print(p.get_content_type(), p.get_filename()) for p in m.walk()]"`.
4. Pull URLs: `grep -Eoi 'https?://[^\"> ]+' inbox-00-normal.eml`.
5. Confirm against the answer: The actionable link is inside a QR image

## MITRE ATT&CK
- T1566.002 Phishing: Spearphishing Link

## Tools
Hand written .eml messages with real headers; the malicious mail sits among normal business mail

## Signature
Forensic Sample Blueprint | qayssarayra.com
