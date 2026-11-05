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

## How to solve it

1. Extract the evidence, password `hidden`: `7z x e120d630fb2c6be6d5399a1e5563533f1bc4a74cf7ee494aca39d1ab8bc605fe.zip -phidden`. You get `udp-flood.pcap`.
2. Open `udp-flood.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `icmp.type==3`  (ICMP port-unreachable replies to UDP probes). Then Statistics > Conversations to see the talkers.
4. Same filter from the shell with tshark: `tshark -r udp-flood.pcap -Y "icmp.type==3"`, and list talkers with `tshark -r udp-flood.pcap -q -z conv,ip`.
5. Confirm against the answer: A very high rate of UDP packets to one host

## MITRE ATT&CK
- T1498.001 Network Denial of Service: Direct Network Flood

## Tools
Crafted UDP flood (scapy) over generated background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
