# ssti

## Scenario
An attacker tests server side template injection on a name parameter, using {{7*7}} style payloads that the template engine evaluates.

## Timeline (UTC+3)
```
15:10:00  SSTI probes on /greet?name= from 45.137.22.90
15:10:xx  {{7*7}}, ${7*7}, {{config}} payloads
```

## How to spot it
- Template expressions ({{ }}, ${ }) in a parameter
- The response shows the expression evaluated (49)
- Reads of template config objects
- Leads to RCE in many engines

## Questions
```
1. Endpoint?          /greet (name param)
2. Attack?            Server side template injection
3. Marker payload?    {{7*7}} -> 49
4. Escalation?        {{config}} and RCE
5. Engines?           Jinja2, Twig, Freemarker, etc
```

## MITRE ATT&CK
- T1190 Exploit Public-Facing Application

## Tools
Hand written Apache access log with real workday background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
