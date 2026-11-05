# Redirector Chain

## Scenario

Traffic is relayed through a chain of redirectors before reaching the real C2: several hops via CDN and mirror hosts, each handing off to the next, hiding the bulletproof backend.

Evidence: `redirector-chain.pcap`  
Capture date: 2024-03-24

## Timeline

~12:00  A chain of HTTP redirects through edge and mirror hosts, ending at a backend beacon.

## How to spot it

- A chain of 302 redirects through multiple hosts
- CDN and mirror style intermediary domains
- Final hop to a low reputation backend
- Infrastructure layering to hide the real C2

## Questions

```
1. Technique?         Redirector chain
2. Hops?               cdn-edge, mirror-eu, backend-ops
3. Final?              backend-ops.example
4. Goal?               hide the real C2
5. ATT&CK?             T1090.002
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x <sha256>.zip -phidden`. You get `redirector-chain.pcap`.
2. Open `redirector-chain.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `http`  (the redirect chain). Then Statistics > Conversations for the talkers.
4. Same filter from the shell: `tshark -r redirector-chain.pcap -Y "http"`, and `tshark -r redirector-chain.pcap -q -z conv,ip`.
5. Confirm against the answer: A chain of 302 redirects through multiple hosts

## MITRE ATT&CK

- T1090.002 External Proxy
- T1583.004 Server

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
