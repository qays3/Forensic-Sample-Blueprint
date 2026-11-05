# webshell-usage

## Scenario
An attacker interacts with a planted webshell, sending commands via repeated POSTs to the same .php file and getting output back.

## Timeline (UTC+3)
```
15:00:00  Repeated POSTs to /uploads/shell.php from 45.137.22.90
15:0x:xx  Each POST runs a command, output sizes vary
```

## How to spot it
- Repeated POSTs to one odd .php file
- Response sizes vary as different commands run
- Traffic to a file that should not take POSTs
- One external IP driving it

## Questions
```
1. Shell?             /uploads/shell.php
2. Method?            POST (commands)
3. Sign?              Varying response sizes
4. Attacker IP?       45.137.22.90
5. Phase?             Webshell interaction
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x d34ee0a54ad5e625c9ccb3ea79083385684d94e1a70108d7631aefb86f6ab20b.zip -phidden` (or your archive tool). You get `access.log`.
2. Find the busiest source: `awk '{print $1}' access.log | sort | uniq -c | sort -rn | head`.
3. Pull the suspect requests and their status codes: `grep -Ei 'select|union|\.\./|<script|/admin|\.php|payload' access.log` (adjust to this attack), and `awk '{print $9}' access.log | sort | uniq -c`.
4. Confirm against the answer: Repeated POSTs to one odd .php file

## MITRE ATT&CK
- T1505.003 Web Shell
- T1059 Command and Scripting Interpreter

## Tools
Hand written Apache access log with real workday background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
