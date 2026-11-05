# Bookmark History Discovery

## Scenario

The browser was pointed at its own History and Bookmarks files via file:// URLs, a sign someone enumerated browsing data on the host.

Host: WS-FIN-07  
Evidence: `History` (Chrome/Edge History SQLite)  
Activity date: 2021-11-02

## Timeline

~02:03  file:// visits to the Chrome History and Bookmarks files.

## How to spot it

- file:// URLs to browser profile files
- Access to History and Bookmarks directly
- Odd hour, single visits
- Recon of the user's browsing

## Questions

```
1. Tell?              file:// to History/Bookmarks
2. What?               Reading browsing data
3. Time?               02:03
4. Goal?               Discovery of habits/sites
5. Technique?          Browser info discovery
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x d894e1ce313a068d06a217af59b7c3c2c628fbbd48e481751df46bd1a97a51ed.zip -phidden` (or your archive tool). You get `History`.
2. Open the SQLite DB: `sqlite3 History`.
3. Visits: `SELECT url,title,visit_count,datetime(last_visit_time/1000000-11644473600,'unixepoch') FROM urls ORDER BY last_visit_time DESC LIMIT 25;`.
4. Downloads: `SELECT target_path,tab_url,referrer FROM downloads;`.
5. Confirm against the answer: file:// URLs to browser profile files

## MITRE ATT&CK

- T1217 Browser Information Discovery

## Tools

DB Browser for SQLite, Hindsight, or `sqlite3`. Chrome timestamps are microseconds since 1601-01-01 UTC.

## Signature

Forensic Sample Blueprint | qayssarayra.com
