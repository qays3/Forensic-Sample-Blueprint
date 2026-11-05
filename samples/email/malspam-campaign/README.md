# malspam-campaign

## Scenario
A mass malspam campaign (Emotet/Qakbot style) uses a delivery failure lure and a zipped payload, sent to many recipients.

## Timeline
```
Parcel delivery lure with a zipped payload, bulk sent
```

## How to spot it
- A delivery/shipping lure with a zip attachment
- Bulk mailer headers (PHPMailer)
- Lookalike courier domain
- Part of a wider campaign to many recipients

## Questions
```
1. Lure?              Parcel delivery failure
2. Attachment?         Shipping_Label.zip
3. Campaign tell?       PHPMailer, many recipients
4. Sender?             dhl-parcel-track.com
5. Family style?        Emotet/Qakbot
```

## MITRE ATT&CK
- T1566.001 Phishing: Spearphishing Attachment

## Tools
Hand written .eml messages with real headers; the malicious mail sits among normal business mail

## Signature
Forensic Sample Blueprint | qayssarayra.com
