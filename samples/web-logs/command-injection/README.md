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

## How to solve it

1. Extract the evidence, password `hidden`: `7z x a5e8405c93095969b4bb3c83e53c519406ea29204f52a7bf7b680399856588d5.zip -phidden` (or your archive tool). You get `access.log`.
2. Find the busiest source: `awk '{print $1}' access.log | sort | uniq -c | sort -rn | head`.
3. Pull the suspect requests and their status codes: `grep -Ei 'select|union|\.\./|<script|/admin|\.php|payload' access.log` (adjust to this attack), and `awk '{print $9}' access.log | sort | uniq -c`.
4. Confirm against the answer: Shell metacharacters (; | ` $()) in a parameter

## MITRE ATT&CK
- T1190 Exploit Public-Facing Application

## Tools
Hand written Apache access log with real workday background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
