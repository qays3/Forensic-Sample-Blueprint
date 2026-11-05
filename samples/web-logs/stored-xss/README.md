# stored-xss

## Scenario
An attacker posts a comment containing a script payload (stored XSS). Later, every visitor who loads the product page runs the attacker script.

## Timeline (UTC+3)
```
12:20:00  POST /api/v1/comments with a script payload (stored)
12:20:xx  GET /products/142 serves the poisoned page to visitors
```

## How to spot it
- A POST that stores user content, carrying a script payload
- Later page loads serve the stored script to other users
- The payload persists, unlike reflected XSS
- Comment or review endpoints are common targets

## Questions
```
1. Store endpoint?    /api/v1/comments
2. XSS type?          Stored
3. Where it fires?    /products/142 for every visitor
4. Difference vs reflected?  It persists server side
5. Payload location?  The stored comment body
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 44529f1312462a3e99d7511e725dfdb29c3e32a67c4efc7ec61e92ca5fd8e194.zip -phidden` (or your archive tool). You get `access.log`.
2. Find the busiest source: `awk '{print $1}' access.log | sort | uniq -c | sort -rn | head`.
3. Pull the suspect requests and their status codes: `grep -Ei 'select|union|\.\./|<script|/admin|\.php|payload' access.log` (adjust to this attack), and `awk '{print $9}' access.log | sort | uniq -c`.
4. Confirm against the answer: A POST that stores user content, carrying a script payload

## MITRE ATT&CK
- T1059.007 JavaScript

## Tools
Hand written Apache access log; the stored script is in the POST body (app log)

## Signature
Forensic Sample Blueprint | qayssarayra.com
