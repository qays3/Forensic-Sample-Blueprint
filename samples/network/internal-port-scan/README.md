# Internal Port Scan

## Scenario

An internal port scan from 10.10.1.66: one host probes a fixed set of ports across the 10.10.2.0/24 range to map services.

Evidence: `internal-port-scan.pcap`  
Capture date: 2019-05-04

## Timeline

~11:13  One source sending SYN to common ports across many internal hosts.

## How to spot it

- One host hitting many internal IPs
- A fixed set of common ports per host
- SYN-ACK on open, RST on closed
- Internal reconnaissance sweep

## Questions

```
1. Scanner?           10.10.1.66
2. Range?               10.10.2.0/24
3. Ports?              22,80,135,445,3389,5985
4. Tell?               one host, many targets
5. Technique?          Network service discovery
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x <sha256>.zip -phidden`. You get `internal-port-scan.pcap`.
2. Open `internal-port-scan.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `tcp.flags.syn==1 && tcp.flags.ack==0`  (the internal sweep). Then Statistics > Conversations for the talkers.
4. Same filter from the shell: `tshark -r internal-port-scan.pcap -Y "tcp.flags.syn==1 && tcp.flags.ack==0"`, and `tshark -r internal-port-scan.pcap -q -z conv,ip`.
5. Confirm against the answer: One host hitting many internal IPs

## MITRE ATT&CK

- T1046 Network Service Discovery
- T1018 Remote System Discovery

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
