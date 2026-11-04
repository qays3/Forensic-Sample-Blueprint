# lfi-path-traversal

## Scenario
An attacker exploits a file download endpoint with path traversal and PHP filter wrappers to read server files like /etc/passwd and the app config.

## Timeline (UTC+3)
```
14:10:00  Path traversal on /download?file= from 45.137.22.90
14:10:xx  ../../etc/passwd, encoded ../, php://filter to read config.php
```

## How to spot it
- ../ sequences (raw and URL encoded) in a file parameter
- php://filter wrapper to base64 read source
- Reads of /etc/passwd, /etc/shadow, config files
- Large 200 responses when a file is read

## Questions
```
1. Endpoint?          /download (file param)
2. Attack?            LFI / path traversal
3. Files targeted?    /etc/passwd, /etc/shadow, config.php
4. Wrapper used?      php://filter base64
5. Encoded variant?   %2f for /
```

## MITRE ATT&CK
- T1190 Exploit Public-Facing Application
- T1083 File and Directory Discovery

## Tools
Hand written Apache access log with real workday background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
