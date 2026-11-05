# icmp-exfil

## Scenario
A host (10.10.1.57) smuggles stolen data out inside ICMP echo request payloads to 91.219.236.17. Each ping carries a 48 byte base64 chunk of the data.

## Timeline (UTC+3)
```
13:20:00  ICMP exfil starts from 10.10.1.57 to 91.219.236.17
13:2x:xx  Many echo requests, each with a base64 data chunk
```

## How to spot it
- ICMP echo requests with data in the payload, not normal ping
- One host sending a long stream of pings to one external IP
- Payloads decode (base64) to the stolen content
- Fixed ICMP id with rising sequence

## Questions
```
1. Host?             10.10.1.57
2. Destination?      91.219.236.17
3. Protocol?         ICMP echo
4. Encoding?         base64 in the payload
5. ICMP id?          777
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 3cb9186569a281bb4725ab8269720090c9b5c8b09b3d037c9901ed38c08f6ff2.zip -phidden`. You get `icmp-exfil.pcap`.
2. Open `icmp-exfil.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `icmp`  (ICMP echo traffic). Then Statistics > Conversations to see the talkers.
4. Same filter from the shell with tshark: `tshark -r icmp-exfil.pcap -Y "icmp"`, and list talkers with `tshark -r icmp-exfil.pcap -q -z conv,ip`.
5. Confirm against the answer: ICMP echo requests with data in the payload, not normal ping

## MITRE ATT&CK
- T1048.003 Exfiltration Over Alternative Protocol
- T1095 Non-Application Layer Protocol

## Tools
Crafted ICMP exfil (scapy) over generated background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
