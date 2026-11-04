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

## MITRE ATT&CK
- T1190 Exploit Public-Facing Application

## Tools
Hand written Apache access log with real workday background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
