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

## How to solve it

1. Extract the evidence, password `hidden`: `7z x a96844bc29cde4a7110ab936f4e810938af7bf4f308c6857db810f1cd2c8af56.zip -phidden`. You get `vlan-hopping.pcap`.
2. Open `vlan-hopping.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `icmp`  (ICMP echo traffic). Then Statistics > Conversations to see the talkers.
4. Same filter from the shell with tshark: `tshark -r vlan-hopping.pcap -Y "icmp"`, and list talkers with `tshark -r vlan-hopping.pcap -q -z conv,ip`.
5. Confirm against the answer: Frames carrying two stacked 802.1Q VLAN tags

## MITRE ATT&CK
- T1599 Network Boundary Bridging

## Tools
Crafted VLAN double tagging (scapy) over generated background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
