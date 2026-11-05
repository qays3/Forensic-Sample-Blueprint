# Firewall Evasion Fragmentation

## Scenario

An attacker split a request into many tiny, overlapping IP fragments so an inspecting firewall or IDS reassembles it differently (or not at all) while the target reassembles the real blocked request.

Evidence: `firewall-evasion-fragmentation.pcap`  
Capture date: 2021-11-09

## Timeline

~12:04  A stream of very small IP fragments with the same id that reassemble into a request designed to slip past inspection.

## How to spot it

- Unusually small IP fragments
- Overlapping or out-of-order fragment offsets
- Payload only meaningful after reassembly
- Used to defeat IDS/firewall signatures

## Questions

```
1. Technique?         Fragmentation evasion
2. Tell?               tiny overlapping fragments
3. Shared?             IP id across fragments
4. Goal?               bypass IDS/firewall
5. ATT&CK?             T1599 / evasion
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 505c8e346db9835372cba33362293865810385be4fb26f98d25528a43ad6b4eb.zip -phidden`. You get `firewall-evasion-fragmentation.pcap`.
2. Open `firewall-evasion-fragmentation.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `tcp.flags.syn==1 && tcp.flags.ack==0`  (SYN probes across ports). Then Statistics > Conversations to see the talkers.
4. Same filter from the shell with tshark: `tshark -r firewall-evasion-fragmentation.pcap -Y "tcp.flags.syn==1 && tcp.flags.ack==0"`, and list talkers with `tshark -r firewall-evasion-fragmentation.pcap -q -z conv,ip`.
5. Confirm against the answer: Unusually small IP fragments

## MITRE ATT&CK

- T1205 Traffic Signaling
- T1599 Network Boundary Bridging

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
