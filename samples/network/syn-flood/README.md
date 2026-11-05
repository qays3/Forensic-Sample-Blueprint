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

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 2f433b575fc454c3b77cdcd7a76a50ace76caa5ebbdf4a34829ac06667f0b07c.zip -phidden`. You get `syn-flood.pcap`.
2. Open `syn-flood.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `ip.addr==10.10.1.66`  (traffic to and from the attacker host). Then Statistics > Conversations to see the talkers.
4. Same filter from the shell with tshark: `tshark -r syn-flood.pcap -Y "ip.addr==10.10.1.66"`, and list talkers with `tshark -r syn-flood.pcap -q -z conv,ip`.
5. Confirm against the answer: A huge rate of SYN packets to one service

## MITRE ATT&CK
- T1498.001 Network Denial of Service: Direct Network Flood

## Tools
Crafted SYN flood (scapy) over generated background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
