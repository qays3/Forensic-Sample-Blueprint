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

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 922449da016d7aa266c45b650ac34bcf3984831698e7ce12d337ea6554450c35.zip -phidden`. You get `reverse-shell-plain.pcap`.
2. Open `reverse-shell-plain.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `dns`  (DNS queries and answers). Then Statistics > Conversations to see the talkers.
4. Same filter from the shell with tshark: `tshark -r reverse-shell-plain.pcap -Y "dns"`, and list talkers with `tshark -r reverse-shell-plain.pcap -q -z conv,ip`.
5. Confirm against the answer: An internal server makes an outbound connection to an odd high port

## MITRE ATT&CK
- T1059.004 Command and Scripting Interpreter: Unix Shell
- T1071 Application Layer Protocol

## Tools
Crafted reverse shell session (scapy) over generated background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
