# tor-traffic

## Scenario
A host (10.10.1.84) connects to the Tor network, reaching known Tor guard relays on port 9001 with TLS. Tor use from a workstation can hide C2 or data theft behind onion routing.

## Timeline (UTC+3)
```
10:00:00  10.10.1.84 connects to Tor guard relays on 9001
10:0x:xx  Repeated TLS sessions to known Tor node IPs
```

## How to spot it
- Connections to IPs on public Tor relay lists
- Port 9001 and Tor style TLS certificates
- A workstation using Tor is unusual on a corporate network
- Destinations rotate across known guard nodes

## Questions
```
1. Host?             10.10.1.84
2. Network?          Tor
3. Port?             9001 (Tor OR port)
4. How to confirm?   Match IPs to the public Tor relay list
5. Why suspicious?   Hides C2 or exfil behind onion routing
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 8b706c69d3ed556c7c0b794d78f8d400a54c320bb99a25b451a11789683bac59.zip -phidden`. You get `tor-traffic.pcap`.
2. Open `tor-traffic.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `http || tls`  (HTTP/TLS requests, hosts and user agents). Then Statistics > Conversations to see the talkers.
4. Same filter from the shell with tshark: `tshark -r tor-traffic.pcap -Y "http || tls"`, and list talkers with `tshark -r tor-traffic.pcap -q -z conv,ip`.
5. Confirm against the answer: Connections to IPs on public Tor relay lists

## MITRE ATT&CK
- T1090.003 Proxy: Multi-hop Proxy

## Tools
Crafted Tor guard traffic (scapy) over generated background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
