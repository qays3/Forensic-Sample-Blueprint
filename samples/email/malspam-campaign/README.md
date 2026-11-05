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

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 36ca4112b87ab04124b675875a538fae689722c668b29125667e6da4abe31e2d.zip -phidden` (or your archive tool). You get `inbox-00-normal.eml`.
2. Read the headers: `cat inbox-00-normal.eml`; check `Received`, `From`, `Reply-To` and `Authentication-Results` (SPF, DKIM, DMARC).
3. List the parts and decode attachments: `python3 -c "import email; m=email.message_from_file(open('inbox-00-normal.eml')); [print(p.get_content_type(), p.get_filename()) for p in m.walk()]"`.
4. Pull URLs: `grep -Eoi 'https?://[^\"> ]+' inbox-00-normal.eml`.
5. Confirm against the answer: A delivery/shipping lure with a zip attachment

## MITRE ATT&CK
- T1566.001 Phishing: Spearphishing Attachment

## Tools
Hand written .eml messages with real headers; the malicious mail sits among normal business mail

## Signature
Forensic Sample Blueprint | qayssarayra.com
