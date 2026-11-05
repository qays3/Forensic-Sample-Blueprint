# tcp-syn-scan

## Scenario
A workstation (10.10.1.66) ran a TCP SYN (half open) port scan against the file server  
10.10.2.15, sweeping ports 1 to 1024. The scan is mixed into a normal workday of web, DNS,  
SMB and NTP traffic from other hosts.  

## Timeline (UTC+3)
```
09:00:00  Capture starts, normal office traffic
12:20:00  SYN scan begins from 10.10.1.66 to 10.10.2.15, source port 40668
12:20:xx  Scan finishes after 1024 probes
```

## How to spot it
- One source hits 1024 destination ports on one host in seconds  
- Low TTL (44) and fixed window (1024) are typical of a crafted scanner  
- Closed ports reply RST ACK, open ports reply SYN ACK then the scanner sends RST (never completes the handshake)  

## Open ports found: 22, 80, 443, 445, 3389

## Questions
```
1. What is the scanner IP?            10.10.1.66
2. What host was scanned?             10.10.2.15
3. How many ports were probed?        1024
4. Which ports are open?              22, 80, 443, 445, 3389
5. Is the handshake completed?        No, scanner sends RST after SYN ACK
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x f2918dccca93943d691897378d43a01f38fb050819a45bbd5fea00b89a26ae94.zip -phidden`. You get `tcp-syn-scan.pcap`.
2. Open `tcp-syn-scan.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `tcp.flags.syn==1 && tcp.flags.ack==0`  (SYN probes across ports). Then Statistics > Conversations to see the talkers.
4. Same filter from the shell with tshark: `tshark -r tcp-syn-scan.pcap -Y "tcp.flags.syn==1 && tcp.flags.ack==0"`, and list talkers with `tshark -r tcp-syn-scan.pcap -q -z conv,ip`.
5. Confirm against the answer: One source hits 1024 destination ports on one host in seconds

## MITRE ATT&CK
T1595.001 Active Scanning: Scanning IP Blocks  
T1046 Network Service Discovery  

## Tools
nmap style SYN scan, crafted with scapy, captured alongside generated background traffic  
Packets: 8066  

## Signature
Forensic Sample Blueprint  |  qayssarayra.com
