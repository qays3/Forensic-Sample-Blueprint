# Chisel Port Forward

## Scenario

A chisel tunnel: a long lived WebSocket to an external host on 8080 carries forwarded traffic, used for pivoting and port forwarding.

Evidence: `chisel-port-forward.pcap`  
Capture date: 2023-09-24

## Timeline

~11:08  A WebSocket upgrade to 45.33.32.156:8080 with chisel protocol, then a long lived tunnel.

## How to spot it

- A WebSocket upgrade to an external host
- chisel protocol token in the handshake
- One long lived connection with steady data
- Used for port forwarding and pivoting

## Questions

```
1. Tool?              chisel
2. Transport?           WebSocket on 8080
3. Tell?               Upgrade + chisel-v3
4. Purpose?            tunnel/port forward
5. Technique?          Protocol tunneling
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x <sha256>.zip -phidden`. You get `chisel-port-forward.pcap`.
2. Open `chisel-port-forward.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `http.upgrade == "websocket" || tcp.port==8080`  (The WebSocket tunnel). Then Statistics > Conversations for the talkers.
4. Same filter from the shell: `tshark -r chisel-port-forward.pcap -Y "http.upgrade == "websocket" || tcp.port==8080"`, and `tshark -r chisel-port-forward.pcap -q -z conv,ip`.
5. Confirm against the answer: A WebSocket upgrade to an external host

## MITRE ATT&CK

- T1572 Protocol Tunneling
- T1090 Proxy

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
