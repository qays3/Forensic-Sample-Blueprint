# Rdp Connection

## Scenario

An RDP connection from 10.10.1.66 to 10.10.2.15: the X.224 request carries an mstshash cookie with the username, then TLS secures the channel.

Evidence: `rdp-connection.pcap`  
Capture date: 2023-05-03

## Timeline

~12:03  RDP X.224 request to 3389 with mstshash=Administrator, then TLS.

## How to spot it

- Connection to RDP port 3389
- X.224 request with an mstshash cookie
- Username visible in the cookie
- TLS wrapped session after

## Questions

```
1. Protocol?          RDP (3389)
2. Tell?               mstshash cookie
3. User?               Administrator
4. Source?             operator host
5. Technique?          RDP lateral movement
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x <sha256>.zip -phidden`. You get `rdp-connection.pcap`.
2. Open `rdp-connection.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `tcp.port==3389`  (the RDP handshake). Then Statistics > Conversations for the talkers.
4. Same filter from the shell: `tshark -r rdp-connection.pcap -Y "tcp.port==3389"`, and `tshark -r rdp-connection.pcap -q -z conv,ip`.
5. Confirm against the answer: Connection to RDP port 3389

## MITRE ATT&CK

- T1021.001 Remote Desktop Protocol

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
