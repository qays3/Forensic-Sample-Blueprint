# defacement

## Scenario
After compromising the server, the attacker overwrites the home page (defacement), replacing index.html with their own content.

## Timeline (UTC+3)
```
15:45:00  Webshell used, then index.html overwritten
15:45:xx  GET / now serves the defaced page
```

## How to spot it
- A webshell POST, then a write to index.html
- A PUT or shell driven change to the home page
- The home page content/size changes
- Defacement follows a compromise

## Questions
```
1. Attack?            Website defacement
2. Precursor?         Webshell (/uploads/shell.php)
3. Page changed?      index.html
4. Confirm?           GET / serves new content
5. Phase?             Impact
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 1b0ef1d94863220437bc38267bf836feeb56711efb2e7d9956e58eac938dd5ab.zip -phidden` (or your archive tool). You get `access.log`.
2. Find the busiest source: `awk '{print $1}' access.log | sort | uniq -c | sort -rn | head`.
3. Pull the suspect requests and their status codes: `grep -Ei 'select|union|\.\./|<script|/admin|\.php|payload' access.log` (adjust to this attack), and `awk '{print $9}' access.log | sort | uniq -c`.
4. Confirm against the answer: A webshell POST, then a write to index.html

## MITRE ATT&CK
- T1491.001 Defacement: Internal Defacement

## Tools
Hand written Apache access log with real workday background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
