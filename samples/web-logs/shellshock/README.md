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

## MITRE ATT&CK
- T1190 Exploit Public-Facing Application

## Tools
Hand written Apache access log with real workday background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
