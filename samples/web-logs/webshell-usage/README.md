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

## MITRE ATT&CK
- T1505.003 Web Shell
- T1059 Command and Scripting Interpreter

## Tools
Hand written Apache access log with real workday background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
