# reverse-shell-plain

## Scenario
A compromised web server (10.10.1.72) opens a reverse shell to the attacker at 194.26.135.9 on port 4444. The session is plaintext, so the commands and their output are readable straight from the capture.

## Timeline (UTC+3)
```
09:43:20  Reverse shell connects from 10.10.1.72 to 194.26.135.9:4444
09:43:xx  Attacker runs id, uname, reads /etc/passwd
```

## How to spot it
- An internal server makes an outbound connection to an odd high port
- Interactive shell commands and output in cleartext
- Outbound to a port like 4444 that no normal service uses
- The server initiates the connection, unusual for a server

## Questions
```
1. Victim?           10.10.1.72
2. Attacker and port?  194.26.135.9:4444
3. Shell direction?  Reverse (victim connects out)
4. First commands?   id, uname -a, cat /etc/passwd
5. Encrypted?        No, plaintext
```

## MITRE ATT&CK
- T1059.004 Command and Scripting Interpreter: Unix Shell
- T1071 Application Layer Protocol

## Tools
Crafted reverse shell session (scapy) over generated background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
