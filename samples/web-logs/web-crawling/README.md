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

## How to solve it

1. Extract the evidence, password `hidden`: `7z x b84d838aa5d8fad6ae82fdf3039a1fb5fb5e867a33cd354e5e0e85d3e11e490e.zip -phidden` (or your archive tool). You get `access.log`.
2. Find the busiest source: `awk '{print $1}' access.log | sort | uniq -c | sort -rn | head`.
3. Pull the suspect requests and their status codes: `grep -Ei 'select|union|\.\./|<script|/admin|\.php|payload' access.log` (adjust to this attack), and `awk '{print $9}' access.log | sort | uniq -c`.
4. Confirm against the answer: One client fetches pages in order, fast

## MITRE ATT&CK
- T1595 Active Scanning

## Tools
Hand written Apache access log with real workday background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
