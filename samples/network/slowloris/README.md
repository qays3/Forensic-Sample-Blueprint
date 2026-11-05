# Slowloris

## Scenario

A Slowloris attack: many connections are opened and kept alive with slow partial HTTP headers, holding the web server's workers open.

Evidence: `slowloris.pcap`  
Capture date: 2025-08-13

## Timeline

~13:00  Many half sent HTTP requests with trickled header lines, connections never completing.

## How to spot it

- Many open connections with partial requests
- Headers sent a few bytes at a time
- Connections held open for a long time
- Low bandwidth, high connection count

## Questions

```
1. Technique?         Slowloris (L7 DoS)
2. Target?             10.10.2.82:80
3. Tell?               partial slow headers
4. Effect?             worker exhaustion
5. Rate?               low bandwidth, many conns
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x <sha256>.zip -phidden`. You get `slowloris.pcap`.
2. Open `slowloris.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `http && tcp.port==80`  (the slow partial requests). Then Statistics > Conversations for the talkers.
4. Same filter from the shell: `tshark -r slowloris.pcap -Y "http && tcp.port==80"`, and `tshark -r slowloris.pcap -q -z conv,ip`.
5. Confirm against the answer: Many open connections with partial requests

## MITRE ATT&CK

- T1499.002 Service Exhaustion Flood

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
