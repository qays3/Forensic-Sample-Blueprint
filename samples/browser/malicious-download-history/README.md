# Malicious Download History

## Scenario

The History and Downloads tables show the user was directed to a fake vendor site and downloaded an executable disguised as an invoice viewer.

Host: WS-FIN-07  
Evidence: `History` (Chrome/Edge History SQLite)  
Activity date: 2022-02-13

## Timeline

~14:03  Typed/visited invoicesoft-update.com, then an .exe downloaded to Downloads from the same site.

## How to spot it

- A download of an .exe from a freshly seen domain
- tab_url and referrer are the same lookalike site
- Download named to look like a document tool
- Sits among normal browsing

## Questions

```
1. Table?             downloads + urls
2. File?               Invoice_Viewer_Setup.exe
3. Source?             invoicesoft-update.com
4. Type?               application/x-msdownload
5. Technique?          Malicious download
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 91e3b8ff24009732a669e8c253deb7078ef58439ecebee213412bb7cbbe668e1.zip -phidden` (or your archive tool). You get `History`.
2. Open the SQLite DB: `sqlite3 History`.
3. Visits: `SELECT url,title,visit_count,datetime(last_visit_time/1000000-11644473600,'unixepoch') FROM urls ORDER BY last_visit_time DESC LIMIT 25;`.
4. Downloads: `SELECT target_path,tab_url,referrer FROM downloads;`.
5. Confirm against the answer: A download of an .exe from a freshly seen domain

## MITRE ATT&CK

- T1105 Ingress Tool Transfer
- T1204.002 Malicious File

## Tools

DB Browser for SQLite, Hindsight, or `sqlite3`. Chrome timestamps are microseconds since 1601-01-01 UTC.

## Signature

Forensic Sample Blueprint | qayssarayra.com
