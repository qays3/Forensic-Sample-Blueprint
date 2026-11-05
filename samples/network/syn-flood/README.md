# Syn Flood

## Scenario

A SYN flood against a web server: a high rate of SYN packets with spoofed sources and no completed handshakes exhausts the connection table.

Evidence: `syn-flood.pcap`  
Capture date: 2024-06-07

## Timeline

~11:00  Thousands of SYN to port 80 from many spoofed sources, no SYN-ACK completion.

## How to spot it

- A very high rate of SYN packets
- Spoofed or many random sources
- No completed three way handshakes
- Half open connections pile up

## Questions

```
1. Technique?         SYN flood
2. Target?             10.10.2.80:80
3. Tell?               SYN only, no handshakes
4. Sources?            many spoofed
5. Effect?             connection table exhaustion
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x <sha256>.zip -phidden`. You get `syn-flood.pcap`.
2. Open `syn-flood.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `tcp.flags.syn==1 && tcp.flags.ack==0`  (the SYN flood). Then Statistics > Conversations for the talkers.
4. Same filter from the shell: `tshark -r syn-flood.pcap -Y "tcp.flags.syn==1 && tcp.flags.ack==0"`, and `tshark -r syn-flood.pcap -q -z conv,ip`.
5. Confirm against the answer: A very high rate of SYN packets

## MITRE ATT&CK

- T1498.001 Direct Network Flood

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
