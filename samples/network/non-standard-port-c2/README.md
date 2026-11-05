# non-standard-port-c2

## Scenario
A host (10.10.1.99) runs plain HTTP command and control to 176.113.115.84 on port 8443. Port 8443 usually means HTTPS, but here it carries cleartext HTTP, a mismatch that gives the channel away.

## Timeline (UTC+3)
```
10:20:00  First HTTP beacon to 176.113.115.84 on port 8443
all day   GET /update about every 2 minutes, ~170 times
```

## How to spot it
- HTTP in cleartext on a port that normally means HTTPS (8443)
- Regular GET /update beacons to one external IP
- curl user agent from a workstation
- Protocol does not match the port, a common C2 tell

## Questions
```
1. Host?             10.10.1.99
2. C2 and port?      176.113.115.84:8443
3. Mismatch?         Cleartext HTTP on an HTTPS port
4. Interval?         ~2 minutes
5. User agent?       curl
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 4332e4ac4a3bba886ea70a02f0e8b67d292963c43d7c4c610108a0ce3043ec4a.zip -phidden`. You get `non-standard-port-c2.pcap`.
2. Open `non-standard-port-c2.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `http || tls`  (HTTP/TLS requests, hosts and user agents). Then Statistics > Conversations to see the talkers.
4. Same filter from the shell with tshark: `tshark -r non-standard-port-c2.pcap -Y "http || tls"`, and list talkers with `tshark -r non-standard-port-c2.pcap -q -z conv,ip`.
5. Confirm against the answer: HTTP in cleartext on a port that normally means HTTPS (8443)

## MITRE ATT&CK
- T1571 Non-Standard Port
- T1071.001 Web Protocols

## Tools
Crafted non standard port C2 (scapy) over generated background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
