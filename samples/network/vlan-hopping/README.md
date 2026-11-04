# vlan-hopping

## Scenario
An attacker (10.10.1.66) performs VLAN hopping by double tagging frames with two 802.1Q headers. The first switch strips the outer tag and forwards the frame into a VLAN the attacker should not reach.

## Timeline (UTC+3)
```
09:50:00  Double tagged frames start from 10.10.1.66
09:50:xx  Outer tag VLAN 1, inner tag VLAN 99, crossing VLAN boundaries
```

## How to spot it
- Frames carrying two stacked 802.1Q VLAN tags
- Traffic reaching a VLAN the source port should not access
- Often aimed at the switch CDP/DTP MAC
- Double tagging is the classic VLAN hop

## Questions
```
1. Attacker?         10.10.1.66
2. Technique?         Double tagging (802.1Q)
3. How many tags?    Two stacked VLAN tags
4. Why it works?     First switch strips outer tag, forwards inner
5. Goal?             Reach a restricted VLAN
```

## MITRE ATT&CK
- T1599 Network Boundary Bridging

## Tools
Crafted VLAN double tagging (scapy) over generated background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
