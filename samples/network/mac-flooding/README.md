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

## MITRE ATT&CK
- T1557.002 Adversary in the Middle
- T1498 Network Denial of Service

## Tools
Crafted MAC flood (scapy) over generated background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
