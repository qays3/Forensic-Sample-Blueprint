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

## How to solve it

1. Extract the evidence, password `hidden`: `7z x b99c21c174b191eb36db7d7583331b5b853bec48df231be54da68cbfde9f8369.zip -phidden` (or your archive tool). You get `History`.
2. Open the SQLite DB: `sqlite3 History`.
3. Visits: `SELECT url,title,visit_count,datetime(last_visit_time/1000000-11644473600,'unixepoch') FROM urls ORDER BY last_visit_time DESC LIMIT 25;`.
4. Downloads: `SELECT target_path,tab_url,referrer FROM downloads;`.
5. Confirm against the answer: Visits to webhook.site / pastebin raw URLs

## MITRE ATT&CK

- T1567.002 Exfiltration to Cloud Storage
- T1048 Exfil Over Web

## Tools

DB Browser for SQLite, Hindsight, or `sqlite3`. Chrome timestamps are microseconds since 1601-01-01 UTC.

## Signature

Forensic Sample Blueprint | qayssarayra.com
