# websocket-c2

## Scenario
A host (10.10.1.95) upgrades an HTTP connection to a WebSocket to 45.61.136.22 (ws-live-feed.com) and keeps a long lived two way channel for command and control, which looks like a normal web app socket.

## Timeline (UTC+3)
```
10:15:00  10.10.1.95 sends an HTTP Upgrade: websocket to ws-live-feed.com
10:15:xx  101 Switching Protocols, then a long lived WebSocket channel
```

## How to spot it
- An HTTP Upgrade: websocket handshake to an unusual host
- A long lived connection carrying framed WebSocket data
- Looks like a web app socket, but the host is a young C2 domain
- Two way, low volume, persistent

## Questions
```
1. Host?             10.10.1.95
2. C2?               45.61.136.22 (ws-live-feed.com)
3. Channel?          WebSocket (HTTP Upgrade)
4. Handshake reply?  101 Switching Protocols
5. Why used?         Persistent two way, looks like a web app
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x cc6db147e0e6db17dff5ec915c6a8f9fbcafa78cf14457347073ecdf4c36f545.zip -phidden`. You get `websocket-c2.pcap`.
2. Open `websocket-c2.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `http || tls`  (HTTP/TLS requests, hosts and user agents). Then Statistics > Conversations to see the talkers.
4. Same filter from the shell with tshark: `tshark -r websocket-c2.pcap -Y "http || tls"`, and list talkers with `tshark -r websocket-c2.pcap -q -z conv,ip`.
5. Confirm against the answer: An HTTP Upgrade: websocket handshake to an unusual host

## MITRE ATT&CK
- T1071.001 Application Layer Protocol: Web Protocols
- T1573 Encrypted Channel

## Tools
Crafted WebSocket C2 (scapy) over generated background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
