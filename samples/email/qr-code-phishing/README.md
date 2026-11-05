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

## MITRE ATT&CK
- T1566.002 Phishing: Spearphishing Link

## Tools
Hand written .eml messages with real headers; the malicious mail sits among normal business mail

## Signature
Forensic Sample Blueprint | qayssarayra.com
