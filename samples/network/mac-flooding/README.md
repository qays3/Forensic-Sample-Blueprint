# mac-flooding

## Scenario
An attacker (10.10.1.66) floods the switch with frames from thousands of fake source MAC addresses to overflow the CAM table. When it overflows, the switch fails open and floods frames to all ports, letting the attacker sniff.

## Timeline (UTC+3)
```
09:45:00  Flood of frames with random source MACs begins
09:45:xx  Thousands of unique MACs in seconds (CAM table overflow)
```

## How to spot it
- A flood of frames each with a different random source MAC
- Thousands of new MAC addresses in a very short time
- Aims to overflow the switch CAM table
- After overflow the switch floods all ports, enabling sniffing

## Questions
```
1. Attacker?         10.10.1.66
2. Target?           Switch CAM table
3. What varies?      Source MAC on each frame
4. Effect?           Switch fails open, floods all ports
5. Attacker goal?    Sniff other ports traffic
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x f7befe357cf7b14f739067d5072fd3f27e2aa96f2af6a1c1c76d51c578cf3343.zip -phidden`. You get `mac-flooding.pcap`.
2. Open `mac-flooding.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `ip.addr==10.10.1.66`  (traffic to and from the attacker host). Then Statistics > Conversations to see the talkers.
4. Same filter from the shell with tshark: `tshark -r mac-flooding.pcap -Y "ip.addr==10.10.1.66"`, and list talkers with `tshark -r mac-flooding.pcap -q -z conv,ip`.
5. Confirm against the answer: A flood of frames each with a different random source MAC

## MITRE ATT&CK
- T1557.002 Adversary in the Middle
- T1498 Network Denial of Service

## Tools
Crafted MAC flood (scapy) over generated background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
