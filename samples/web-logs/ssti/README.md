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

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 485ab5aaf7139efbe5a0323bfbcdc64db5d2678e0a5c7cb0746da0ef930cc0c8.zip -phidden` (or your archive tool). You get `access.log`.
2. Find the busiest source: `awk '{print $1}' access.log | sort | uniq -c | sort -rn | head`.
3. Pull the suspect requests and their status codes: `grep -Ei 'select|union|\.\./|<script|/admin|\.php|payload' access.log` (adjust to this attack), and `awk '{print $9}' access.log | sort | uniq -c`.
4. Confirm against the answer: Template expressions ({{ }}, ${ }) in a parameter

## MITRE ATT&CK
- T1190 Exploit Public-Facing Application

## Tools
Hand written Apache access log with real workday background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
