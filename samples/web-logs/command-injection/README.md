# command-injection

## Scenario
An attacker abuses a ping feature at /api/v1/ping, appending shell metacharacters to run commands on the server (id, whoami, cat /etc/passwd).

## Timeline (UTC+3)
```
14:05:00  Command injection on /api/v1/ping?host= from 45.137.22.90
14:05:xx  ; | and backtick payloads running id, whoami, cat /etc/passwd
```

## How to spot it
- Shell metacharacters (; | ` $()) in a parameter
- A feature that runs a system command (ping) abused
- Output size jumps when /etc/passwd is read
- One IP trying several injection styles

## Questions
```
1. Endpoint?          /api/v1/ping (host param)
2. Attack?            OS command injection
3. Metacharacters?    ; | ` 
4. Commands run?      id, whoami, cat /etc/passwd
5. Success sign?      Large response (passwd contents)
```

## MITRE ATT&CK
- T1190 Exploit Public-Facing Application

## Tools
Hand written Apache access log with real workday background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
