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

## MITRE ATT&CK

- T1205 Traffic Signaling
- T1599 Network Boundary Bridging

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
