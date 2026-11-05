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

## MITRE ATT&CK

- T1217 Browser Information Discovery

## Tools

DB Browser for SQLite, Hindsight, or `sqlite3`. Chrome timestamps are microseconds since 1601-01-01 UTC.

## Signature

Forensic Sample Blueprint | qayssarayra.com
