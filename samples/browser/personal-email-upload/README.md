# Personal Email Upload

## Scenario

History shows the user switched to a personal Gmail/Drive profile (u/1) during work hours around the time company files went missing, an insider exfil lead.

Host: WS-FIN-07  
Evidence: `History` (Chrome/Edge History SQLite)  
Activity date: 2025-01-24

## Timeline

~18:20  Logged into a personal Gmail (profile u/1) and personal Drive, distinct from the work account (u/0).

## How to spot it

- A second Google profile (u/1) in history
- Personal mail/drive used on a work host
- Timing lines up with data handling
- Upload channel not logged by corporate DLP

## Questions

```
1. Tell?              Google profile u/1 (personal)
2. Services?           Gmail, Drive
3. On?                 A work machine
4. Risk?               Company files to personal cloud
5. Technique?          Insider exfil via personal email
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 03cefba0c6da6b7ab74e45a4875dcb1e997ad2004563bc4482b5f33bf9bd28e7.zip -phidden` (or your archive tool). You get `History`.
2. Open the SQLite DB: `sqlite3 History`.
3. Visits: `SELECT url,title,visit_count,datetime(last_visit_time/1000000-11644473600,'unixepoch') FROM urls ORDER BY last_visit_time DESC LIMIT 25;`.
4. Downloads: `SELECT target_path,tab_url,referrer FROM downloads;`.
5. Confirm against the answer: A second Google profile (u/1) in history

## MITRE ATT&CK

- T1567.002 Exfiltration to Cloud Storage

## Tools

DB Browser for SQLite, Hindsight, or `sqlite3`. Chrome timestamps are microseconds since 1601-01-01 UTC.

## Signature

Forensic Sample Blueprint | qayssarayra.com
