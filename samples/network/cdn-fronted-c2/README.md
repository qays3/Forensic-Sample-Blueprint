# Cdn Fronted C2

## Scenario

Domain fronted C2 through a CDN: the TLS SNI is a legit CloudFront host but the inner HTTP Host header points at the real C2 backend.

Evidence: `cdn-fronted-c2.pcap`  
Capture date: 2020-12-10

## Timeline

~12:03  TLS to a CloudFront edge with SNI cloudfront.net but an inner Host of the C2 backend.

## How to spot it

- SNI is a trusted CDN host
- Inner HTTP Host differs from the SNI
- Beacons to a CDN edge IP
- Fronting hides the true destination

## Questions

```
1. Technique?         Domain/CDN fronting
2. SNI?                cloudfront.net (trusted)
3. Inner Host?          malicious-c2-backend.xyz
4. Tell?               Host != SNI
5. ATT&CK?             T1090.004
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x <sha256>.zip -phidden`. You get `cdn-fronted-c2.pcap`.
2. Open `cdn-fronted-c2.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `tls.handshake.extensions_server_name contains "cloudfront"`  (Fronted TLS to the CDN). Then Statistics > Conversations for the talkers.
4. Same filter from the shell: `tshark -r cdn-fronted-c2.pcap -Y "tls.handshake.extensions_server_name contains "cloudfront""`, and `tshark -r cdn-fronted-c2.pcap -q -z conv,ip`.
5. Confirm against the answer: SNI is a trusted CDN host

## MITRE ATT&CK

- T1090.004 Domain Fronting
- T1071.001 Web Protocols

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
