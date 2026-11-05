# Paste Site Exfil

## Scenario

Browser history shows repeated visits to a webhook.site endpoint and a pastebin raw URL, consistent with small-volume exfil through a browser.

Host: WS-FIN-07  
Evidence: `History` (Chrome/Edge History SQLite)  
Activity date: 2019-05-14

## Timeline

~23:14  A pastebin raw paste, then several hits to a webhook.site token endpoint.

## How to spot it

- Visits to webhook.site / pastebin raw URLs
- Multiple hits to one webhook token
- Late night, outside normal browsing
- Common low effort exfil channels

## Questions

```
1. Sites?             pastebin, webhook.site
2. Pattern?            Repeated webhook hits
3. Time?               Late night
4. Purpose?            Small data exfil
5. Technique?          Exfil to web service
```

## MITRE ATT&CK

- T1567.002 Exfiltration to Cloud Storage
- T1048 Exfil Over Web

## Tools

DB Browser for SQLite, Hindsight, or `sqlite3`. Chrome timestamps are microseconds since 1601-01-01 UTC.

## Signature

Forensic Sample Blueprint | qayssarayra.com
