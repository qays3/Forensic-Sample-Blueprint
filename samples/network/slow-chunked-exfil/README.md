# slow-chunked-exfil

## Scenario
A host (10.10.1.60) leaks data slowly, sending a small 256 byte chunk every 10 minutes to metrics-collector.net (45.142.212.61). Low and slow exfil stays under volume based alerts.

## Timeline (UTC+3)
```
09:10:00  First small chunk to metrics-collector.net (45.142.212.61)
all day   A small POST every ~10 minutes, ~48 chunks
```

## How to spot it
- Small, regular outbound posts spread over hours
- Deliberately low volume to dodge data loss thresholds
- One host, one external host, fixed small size
- Adds up to a large transfer over the day

## Questions
```
1. Host?             10.10.1.60
2. Destination?      45.142.212.61 (metrics-collector.net)
3. Pattern?          Low and slow, one chunk every 10 min
4. Chunk size?       256 bytes
5. Why slow?         Stay under volume alerts
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 1e2e890e0b8bdf322eff5408083af6bfaa492bc89178733ff88a8832d4376d59.zip -phidden`. You get `slow-chunked-exfil.pcap`.
2. Open `slow-chunked-exfil.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `ip.addr==10.10.1.66`  (traffic to and from the attacker host). Then Statistics > Conversations to see the talkers.
4. Same filter from the shell with tshark: `tshark -r slow-chunked-exfil.pcap -Y "ip.addr==10.10.1.66"`, and list talkers with `tshark -r slow-chunked-exfil.pcap -q -z conv,ip`.
5. Confirm against the answer: Small, regular outbound posts spread over hours

## MITRE ATT&CK
- T1030 Data Transfer Size Limits
- T1041 Exfiltration Over C2 Channel

## Tools
Crafted low and slow exfil (scapy) over generated background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
