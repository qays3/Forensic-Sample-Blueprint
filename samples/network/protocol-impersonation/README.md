# Protocol Impersonation

## Scenario

Traffic on port 443 that is not valid TLS: a custom protocol pretends to be HTTPS to slip past port based rules.

Evidence: `protocol-impersonation.pcap`  
Capture date: 2019-03-20

## Timeline

~12:10  Port 443 traffic with no TLS handshake, raw custom bytes both ways.

## How to spot it

- Port 443 but no TLS ClientHello
- Raw non TLS bytes on an HTTPS port
- Custom framing both directions
- Protocol does not match the port

## Questions

```
1. Port?              443
2. Tell?               no TLS handshake
3. Reality?            custom protocol
4. Goal?               evade port rules
5. Technique?          Protocol impersonation
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x <sha256>.zip -phidden`. You get `protocol-impersonation.pcap`.
2. Open `protocol-impersonation.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `tcp.port==443 && !tls`  (Non TLS traffic on 443). Then Statistics > Conversations for the talkers.
4. Same filter from the shell: `tshark -r protocol-impersonation.pcap -Y "tcp.port==443 && !tls"`, and `tshark -r protocol-impersonation.pcap -q -z conv,ip`.
5. Confirm against the answer: Port 443 but no TLS ClientHello

## MITRE ATT&CK

- T1095 Non-Application Layer Protocol
- T1571 Non-Standard Port

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
