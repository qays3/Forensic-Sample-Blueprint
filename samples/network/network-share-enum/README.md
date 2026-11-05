# Network Share Enum

## Scenario

Network share enumeration from 10.10.1.66: SRVSVC NetShareEnumAll over SMB against many hosts to map accessible shares.

Evidence: `network-share-enum.pcap`  
Capture date: 2019-01-08

## Timeline

~13:06  NetShareEnumAll over SMB against a sweep of internal hosts.

## How to spot it

- NetShareEnumAll over the srvsvc pipe
- One host querying shares on many targets
- Share lists returned for each host
- Mapping where data lives

## Questions

```
1. Call?              NetShareEnumAll
2. Protocol?           SMB srvsvc
3. Pattern?            one host, many targets
4. Goal?               find shares
5. Technique?          Network share discovery
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x <sha256>.zip -phidden`. You get `network-share-enum.pcap`.
2. Open `network-share-enum.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `tcp.port==445`  (the share enumeration). Then Statistics > Conversations for the talkers.
4. Same filter from the shell: `tshark -r network-share-enum.pcap -Y "tcp.port==445"`, and `tshark -r network-share-enum.pcap -q -z conv,ip`.
5. Confirm against the answer: NetShareEnumAll over the srvsvc pipe

## MITRE ATT&CK

- T1135 Network Share Discovery

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
