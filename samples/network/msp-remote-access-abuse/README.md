# Msp Remote Access Abuse

## Scenario

An MSP's remote management channel is abused for intrusion: an RMM agent session comes from the MSP's IP 66.70.100.20 into the environment, a trusted relationship turned into access.

Evidence: `msp-remote-access-abuse.pcap`  
Capture date: 2023-08-21

## Timeline

~10:03  An inbound RMM agent session from an MSP IP, then a stream of encrypted commands.

## How to spot it

- An RMM agent session from a third party (MSP) IP
- A trusted remote management channel
- Commands flowing in from outside
- Trusted relationship abused for access

## Questions

```
1. Technique?         Trusted relationship
2. Source?             MSP IP 66.70.100.20
3. Channel?            RMM agent over TLS
4. Abuse?               management access
5. ATT&CK?             T1199
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x <sha256>.zip -phidden`. You get `msp-remote-access-abuse.pcap`.
2. Open `msp-remote-access-abuse.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `tls || ip.addr==66.70.100.20`  (the MSP RMM session). Then Statistics > Conversations for the talkers.
4. Same filter from the shell: `tshark -r msp-remote-access-abuse.pcap -Y "tls || ip.addr==66.70.100.20"`, and `tshark -r msp-remote-access-abuse.pcap -q -z conv,ip`.
5. Confirm against the answer: An RMM agent session from a third party (MSP) IP

## MITRE ATT&CK

- T1199 Trusted Relationship
- T1219 Remote Access Software

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
