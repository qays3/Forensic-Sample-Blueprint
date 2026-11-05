# Ping Sweep Internal

## Scenario

An internal ping sweep from 10.10.1.66: ICMP echo requests across the subnet to find live hosts.

Evidence: `ping-sweep-internal.pcap`  
Capture date: 2019-09-11

## Timeline

~12:15  ICMP echo to every host in 10.10.2.0/24, replies from the live ones.

## How to spot it

- ICMP echo to a whole subnet from one host
- Sequential destination addresses
- Replies only from live hosts
- Host discovery sweep

## Questions

```
1. Scanner?           10.10.1.66
2. Protocol?           ICMP echo
3. Range?               10.10.2.0/24
4. Live hosts?          those that reply
5. Technique?          Remote system discovery
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x <sha256>.zip -phidden`. You get `ping-sweep-internal.pcap`.
2. Open `ping-sweep-internal.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `icmp`  (the ping sweep). Then Statistics > Conversations for the talkers.
4. Same filter from the shell: `tshark -r ping-sweep-internal.pcap -Y "icmp"`, and `tshark -r ping-sweep-internal.pcap -q -z conv,ip`.
5. Confirm against the answer: ICMP echo to a whole subnet from one host

## MITRE ATT&CK

- T1018 Remote System Discovery

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
