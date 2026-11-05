# shellshock

## Scenario
An attacker sends a Shellshock payload in the User-Agent to a CGI endpoint, running shell commands through the Bash environment bug.

## Timeline (UTC+3)
```
15:25:00  GET /cgi-bin/status with () { :; }; payload in User-Agent
15:25:xx  Command runs via the CGI Bash
```

## How to spot it
- () { :; }; function definition in the User-Agent
- Requests to /cgi-bin/ endpoints
- Commands appended after the empty function
- Classic Shellshock (CVE-2014-6271)

## Questions
```
1. Attack?            Shellshock
2. Payload location?  User-Agent header
3. Signature?         () { :; };
4. Endpoint?          /cgi-bin/status
5. Command?           /bin/bash -c 'id'
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 9f6d78048752f4e29d3efb9545dfffc7c8fd2e1a43c5e2cba37d2167296616e0.zip -phidden` (or your archive tool). You get `access.log`.
2. Find the busiest source: `awk '{print $1}' access.log | sort | uniq -c | sort -rn | head`.
3. Pull the suspect requests and their status codes: `grep -Ei 'select|union|\.\./|<script|/admin|\.php|payload' access.log` (adjust to this attack), and `awk '{print $9}' access.log | sort | uniq -c`.
4. Confirm against the answer: () { :; }; function definition in the User-Agent

## MITRE ATT&CK
- T1190 Exploit Public-Facing Application

## Tools
Hand written Apache access log with real workday background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
