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

## MITRE ATT&CK
- T1566.002 Phishing: Spearphishing Link

## Tools
Hand written .eml messages with real headers; the malicious mail sits among normal business mail

## Signature
Forensic Sample Blueprint | qayssarayra.com
