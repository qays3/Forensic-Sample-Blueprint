# Udp Flood

## Scenario

A UDP flood: a high rate of large UDP packets to a target saturates its bandwidth.

Evidence: `udp-flood.pcap`  
Capture date: 2022-12-14

## Timeline

~12:00  A burst of large UDP packets to one host at a very high rate.

## How to spot it

- A very high UDP packet rate to one target
- Large packet sizes
- Many spoofed sources
- Bandwidth saturation

## Questions

```
1. Technique?         UDP flood
2. Target?             10.10.2.81
3. Tell?               high rate large UDP
4. Sources?            spoofed
5. Effect?             bandwidth saturation
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x <sha256>.zip -phidden`. You get `udp-flood.pcap`.
2. Open `udp-flood.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `udp && ip.dst==10.10.2.81`  (the UDP flood). Then Statistics > Conversations for the talkers.
4. Same filter from the shell: `tshark -r udp-flood.pcap -Y "udp && ip.dst==10.10.2.81"`, and `tshark -r udp-flood.pcap -q -z conv,ip`.
5. Confirm against the answer: A very high UDP packet rate to one target

## MITRE ATT&CK

- T1498.001 Direct Network Flood

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
