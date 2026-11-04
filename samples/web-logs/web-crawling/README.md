# web-crawling

## Scenario
A crawler walks the whole site quickly and sequentially. Here it is aggressive and ignores robots, hitting every page in order.

## Timeline (UTC+3)
```
10:00:00  Sequential crawl of all pages from 66.249.66.1
10:0x:xx  Fast ordered page hits
```

## How to spot it
- One client fetches pages in order, fast
- No asset loads between pages, unlike a browser
- Sequential, broad coverage
- Can be a search bot or a scraper

## Questions
```
1. Crawler IP?        66.249.66.1
2. Behaviour?         Sequential full site crawl
3. Browser or bot?    Bot (no asset loads)
4. Pages hit?         All of them
5. Speed?             Much faster than a human
```

## MITRE ATT&CK
- T1595 Active Scanning

## Tools
Hand written Apache access log with real workday background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
