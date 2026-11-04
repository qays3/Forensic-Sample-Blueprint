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

## MITRE ATT&CK
- T1491.001 Defacement: Internal Defacement

## Tools
Hand written Apache access log with real workday background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
