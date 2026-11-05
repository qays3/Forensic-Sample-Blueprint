# http-request-smuggling

## Scenario
An attacker sends requests with conflicting Content-Length and Transfer-Encoding headers to smuggle a hidden request past the front end proxy.

## Timeline (UTC+3)
```
15:30:00  POSTs with both Content-Length and Transfer-Encoding
15:30:xx  Desync causes an odd 400 and a smuggled request
```

## How to spot it
- Requests carrying both Content-Length and Transfer-Encoding
- Odd 400s and responses that belong to another request
- Front end and back end disagree on request boundaries
- The smuggled request is hidden in the body

## Questions
```
1. Attack?            HTTP request smuggling
2. Header conflict?   Content-Length vs Transfer-Encoding
3. Effect?            Request desync
4. Sign?              Unexpected 400s, mismatched responses
5. Where?             Body of the POST
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 9a3d5c7debc1109b2f3e875b30081fdf12eeb05ac7cb511f0185196eb12cedd7.zip -phidden` (or your archive tool). You get `access.log`.
2. Find the busiest source: `awk '{print $1}' access.log | sort | uniq -c | sort -rn | head`.
3. Pull the suspect requests and their status codes: `grep -Ei 'select|union|\.\./|<script|/admin|\.php|payload' access.log` (adjust to this attack), and `awk '{print $9}' access.log | sort | uniq -c`.
4. Confirm against the answer: Requests carrying both Content-Length and Transfer-Encoding

## MITRE ATT&CK
- T1190 Exploit Public-Facing Application

## Tools
Hand written Apache access log with real workday background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
