# reflected-xss

## Scenario
An attacker tries reflected cross site scripting on the /search q parameter, injecting script and event handler payloads that the page echoes back.

## Timeline (UTC+3)
```
12:15:00  XSS payloads in /search?q= from 45.137.22.90
12:15:xx  script, img onerror, svg onload payloads
```

## How to spot it
- Script tags and event handlers in a query parameter
- Payloads reflected on the search results page
- document.cookie theft attempt
- One IP testing several XSS variants

## Questions
```
1. Parameter?         q on /search
2. XSS type?          Reflected
3. Payloads?          <script>, img onerror, svg onload
4. Goal?              Steal document.cookie
5. Reflected where?   Search results page
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x a4ca168d2bc4c1189c25d10a64f0e1266076d5d76eaca647cbf6d9f50792d87c.zip -phidden` (or your archive tool). You get `access.log`.
2. Find the busiest source: `awk '{print $1}' access.log | sort | uniq -c | sort -rn | head`.
3. Pull the suspect requests and their status codes: `grep -Ei 'select|union|\.\./|<script|/admin|\.php|payload' access.log` (adjust to this attack), and `awk '{print $9}' access.log | sort | uniq -c`.
4. Confirm against the answer: Script tags and event handlers in a query parameter

## MITRE ATT&CK
- T1059.007 Command and Scripting Interpreter: JavaScript

## Tools
Hand written Apache access log with real workday background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
