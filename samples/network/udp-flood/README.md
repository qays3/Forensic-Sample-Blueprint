# udp-flood

## Scenario
The host 10.10.2.20 is hit with a UDP flood: thousands of large UDP packets from spoofed sources to random ports, saturating the link and CPU.

## Timeline (UTC+3)
```
10:20:00  UDP flood begins against 10.10.2.20
10:20:xx  Thousands of 512 byte UDP packets from random sources
```

## How to spot it
- A very high rate of UDP packets to one host
- Random destination ports and spoofed sources
- Large payloads to saturate bandwidth
- Target or link becomes overwhelmed

## Questions
```
1. Target?           10.10.2.20
2. Flood type?       UDP flood
3. Source IPs?       Random, spoofed
4. Payload size?     ~512 bytes
5. Category?         Denial of service
```

## MITRE ATT&CK
- T1498.001 Network Denial of Service: Direct Network Flood

## Tools
Crafted UDP flood (scapy) over generated background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
