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

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 000c091be802021a160147aa51fc8b16fba895cae9b94f8c3f0e9f60c1f9cbcf.zip -phidden`. You get `port-knocking-covert.pcap`.
2. Open `port-knocking-covert.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `http || tls`  (HTTP/TLS requests, hosts and user agents). Then Statistics > Conversations to see the talkers.
4. Same filter from the shell with tshark: `tshark -r port-knocking-covert.pcap -Y "http || tls"`, and list talkers with `tshark -r port-knocking-covert.pcap -q -z conv,ip`.
5. Confirm against the answer: SYNs to a fixed sequence of closed ports

## MITRE ATT&CK

- T1205.001 Port Knocking

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
