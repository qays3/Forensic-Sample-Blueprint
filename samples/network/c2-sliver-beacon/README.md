# C2 Sliver Beacon

## Scenario

The victim 10.10.1.40 beacons over HTTPS to a Sliver style C2 at 185.220.101.9 on a near fixed 60s interval with a self signed certificate and a stable JA3 hash.

Evidence: `c2-sliver-beacon.pcap`  
Capture date: 2023-05-12

## Timeline

~12:00  Regular HTTPS beacons to 185.220.101.9, same JA3, self signed cert, 60s apart.

## How to spot it

- Regular interval TLS to one external IP
- Self signed certificate and odd SNI
- Stable JA3 fingerprint
- Beacon sized requests

## Questions

```
1. C2?                185.220.101.9:443
2. Interval?            ~60s
3. Cert?               self signed
4. Fingerprint?         constant JA3
5. Technique?          C2 framework (Sliver)
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x <sha256>.zip -phidden`. You get `c2-sliver-beacon.pcap`.
2. Open `c2-sliver-beacon.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `ip.addr==185.220.101.9 && tcp.port==443`  (TLS beacons to the C2). Then Statistics > Conversations for the talkers.
4. Same filter from the shell: `tshark -r c2-sliver-beacon.pcap -Y "ip.addr==185.220.101.9 && tcp.port==443"`, and `tshark -r c2-sliver-beacon.pcap -q -z conv,ip`.
5. Confirm against the answer: Regular interval TLS to one external IP

## MITRE ATT&CK

- T1071.001 Web Protocols
- T1573 Encrypted Channel

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
