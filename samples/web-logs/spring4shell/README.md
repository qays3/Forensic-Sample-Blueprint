# spring4shell

## Scenario
An attacker exploits Spring4Shell, POSTing class.module.classLoader parameters to drop a JSP webshell, then requests it.

## Timeline (UTC+3)
```
15:20:00  POST with class.module.classLoader.* params
15:20:xx  GET /tomcatwar.jsp (dropped shell)
```

## How to spot it
- POST params referencing class.module.classLoader
- A JSP file appearing and being requested after
- Spring4Shell writes a webshell via classloader abuse
- The follow up GET of the .jsp confirms it

## Questions
```
1. Attack?            Spring4Shell
2. Param pattern?     class.module.classLoader.*
3. Dropped shell?     /tomcatwar.jsp
4. Confirm?           GET of the jsp returns 200
5. Tech?              Spring / Tomcat
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x d91f221b908bf7dc2bf18b9510fb2b35694dda20fa7d1070389bc14bdcc026ab.zip -phidden` (or your archive tool). You get `access.log`.
2. Find the busiest source: `awk '{print $1}' access.log | sort | uniq -c | sort -rn | head`.
3. Pull the suspect requests and their status codes: `grep -Ei 'select|union|\.\./|<script|/admin|\.php|payload' access.log` (adjust to this attack), and `awk '{print $9}' access.log | sort | uniq -c`.
4. Confirm against the answer: POST params referencing class.module.classLoader

## MITRE ATT&CK
- T1190 Exploit Public-Facing Application

## Tools
Hand written Apache access log with real workday background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
