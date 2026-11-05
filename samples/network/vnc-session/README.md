# Vnc Session

## Scenario

A VNC session from 10.10.1.66 to 10.10.2.17: the RFB handshake on 5900 then framebuffer traffic, remote control often used for lateral movement.

Evidence: `vnc-session.pcap`  
Capture date: 2024-04-20

## Timeline

~11:08  RFB handshake on 5900, auth, then framebuffer updates.

## How to spot it

- Connection to VNC port 5900
- RFB protocol banner
- Auth challenge and response
- Framebuffer update traffic

## Questions

```
1. Protocol?          VNC (RFB) 5900
2. Tell?               RFB banner
3. Use?                remote control
4. Source?             operator host
5. Technique?          Remote services
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x <sha256>.zip -phidden`. You get `vnc-session.pcap`.
2. Open `vnc-session.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `tcp.port==5900`  (the RFB handshake). Then Statistics > Conversations for the talkers.
4. Same filter from the shell: `tshark -r vnc-session.pcap -Y "tcp.port==5900"`, and `tshark -r vnc-session.pcap -q -z conv,ip`.
5. Confirm against the answer: Connection to VNC port 5900

## MITRE ATT&CK

- T1021.005 VNC

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
