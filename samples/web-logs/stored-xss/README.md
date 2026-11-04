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

## MITRE ATT&CK
- T1059.007 JavaScript

## Tools
Hand written Apache access log; the stored script is in the POST body (app log)

## Signature
Forensic Sample Blueprint | qayssarayra.com
