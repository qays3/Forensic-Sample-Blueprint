# syn-flood

## Scenario
The web server 10.10.2.20 is hit with a SYN flood: thousands of SYN packets from spoofed source IPs, filling the connection table with half open connections so real users cannot connect.

## Timeline (UTC+3)
```
10:15:00  SYN flood begins against 10.10.2.20 port 80
10:15:xx  Thousands of SYNs from random spoofed source IPs
```

## How to spot it
- A huge rate of SYN packets to one service
- Source IPs are random and spoofed, no handshake completes
- Many half open connections pile up
- Target becomes unreachable (DoS)

## Questions
```
1. Target?           10.10.2.20 port 80
2. Flood type?       TCP SYN flood
3. Source IPs?       Random, spoofed
4. Effect?           Half open connections exhaust the server
5. Category?         Denial of service
```

## MITRE ATT&CK
- T1498.001 Network Denial of Service: Direct Network Flood

## Tools
Crafted SYN flood (scapy) over generated background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
