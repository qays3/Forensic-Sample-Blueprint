# Rmm As C2

## Scenario

A remote management tool (ScreenConnect) is used as the C2 channel: steady TLS keepalives to a ScreenConnect relay.

Evidence: `rmm-as-c2.pcap`  
Capture date: 2025-11-14

## Timeline

~13:01  Regular TLS to a ScreenConnect relay host acting as remote access C2.

## How to spot it

- TLS to a known RMM relay (ScreenConnect, AnyDesk)
- Steady keepalive beacons
- Legit tool abused for access
- Not an approved admin tool on this host

## Questions

```
1. Tool?              ScreenConnect (RMM)
2. Use?                remote access C2
3. Tell?               RMM relay SNI, keepalives
4. Approved?            no
5. Technique?          Remote access software
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x <sha256>.zip -phidden`. You get `rmm-as-c2.pcap`.
2. Open `rmm-as-c2.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `tls.handshake.extensions_server_name contains "screenconnect"`  (RMM relay traffic). Then Statistics > Conversations for the talkers.
4. Same filter from the shell: `tshark -r rmm-as-c2.pcap -Y "tls.handshake.extensions_server_name contains "screenconnect""`, and `tshark -r rmm-as-c2.pcap -q -z conv,ip`.
5. Confirm against the answer: TLS to a known RMM relay (ScreenConnect, AnyDesk)

## MITRE ATT&CK

- T1219 Remote Access Software

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
