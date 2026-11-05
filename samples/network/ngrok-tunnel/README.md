# ngrok-tunnel

## Scenario
A host (10.10.1.88) opens an ngrok tunnel, connecting to an *.ngrok.io endpoint over TLS. ngrok exposes internal services to the internet or gives an attacker a stable tunnel, bypassing inbound firewall rules.

## Timeline (UTC+3)
```
10:05:00  10.10.1.88 resolves tunnel.ngrok.io and opens a TLS tunnel
10:05:xx  Long lived session to the ngrok edge (3.134.125.175)
```

## How to spot it
- DNS lookup and TLS SNI for an ngrok.io domain
- A long lived tunnel to the ngrok edge infrastructure
- Tunneling services bypass inbound firewall controls
- Unexpected on a managed corporate host

## Questions
```
1. Host?             10.10.1.88
2. Service?          ngrok tunnel
3. Domain?           tunnel.ngrok.io
4. Port?             443 (TLS)
5. Risk?             Exposes internal services or stable attacker tunnel
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x aacc00f77697a7ab876f0c93b6773a018fe0ca6becd0969835a679a473ff448e.zip -phidden`. You get `ngrok-tunnel.pcap`.
2. Open `ngrok-tunnel.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `http || tls`  (HTTP/TLS requests, hosts and user agents). Then Statistics > Conversations to see the talkers.
4. Same filter from the shell with tshark: `tshark -r ngrok-tunnel.pcap -Y "http || tls"`, and list talkers with `tshark -r ngrok-tunnel.pcap -q -z conv,ip`.
5. Confirm against the answer: DNS lookup and TLS SNI for an ngrok.io domain

## MITRE ATT&CK
- T1572 Protocol Tunneling
- T1090 Proxy

## Tools
Crafted ngrok tunnel (scapy) over generated background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
