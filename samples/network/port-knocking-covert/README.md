# Port Knocking Covert

## Scenario

A host sent a specific sequence of SYNs to closed ports (a knock sequence); immediately after the right sequence, a hidden SSH service begins accepting connections, a port knocking covert channel.

Evidence: `port-knocking-covert.pcap`  
Capture date: 2020-02-13

## Timeline

~11:03  SYNs to 7000, 8000, 9000 (all RST), then port 22 suddenly answers SYN-ACK.

## How to spot it

- SYNs to a fixed sequence of closed ports
- All knocks get RST (ports look closed)
- A service opens only after the exact sequence
- Same source for the knocks and the login

## Questions

```
1. Technique?         Port knocking
2. Sequence?           7000, 8000, 9000
3. Before?             22 closed
4. After?              22 opens (SSH)
5. Purpose?            Hidden service access
```

## MITRE ATT&CK

- T1205.001 Port Knocking

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
