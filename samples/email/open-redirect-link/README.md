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

## MITRE ATT&CK
- T1566.002 Phishing: Spearphishing Link

## Tools
Hand written .eml messages with real headers; the malicious mail sits among normal business mail

## Signature
Forensic Sample Blueprint | qayssarayra.com
