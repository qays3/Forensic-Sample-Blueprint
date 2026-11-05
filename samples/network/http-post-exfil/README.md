# http-post-exfil

## Scenario
A host (10.10.1.55) uploads stolen data to an external server (185.220.101.45, upload-sync-files.com) with a run of large HTTP POST requests. The bodies are base64 blobs of the data being stolen.

## Timeline (UTC+3)
```
13:00:00  First large POST to upload-sync-files.com (185.220.101.45)
13:0x:xx  20 POSTs, each ~1.6KB base64 body, data leaving
```

## How to spot it
- Repeated large HTTP POST bodies to one external host
- Outbound volume much larger than the responses
- Young domain only this host uploads to
- Base64 blobs in the request bodies

## Questions
```
1. Host?             10.10.1.55
2. Destination?      185.220.101.45 (upload-sync-files.com)
3. Method?           HTTP POST with base64 bodies
4. How many uploads? 20
5. Direction skew?   Large outbound, tiny responses
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 9773e00ca68d6e1e967bb8c41d418368f578aa39b86cfc5a7f15727467934661.zip -phidden`. You get `http-post-exfil.pcap`.
2. Open `http-post-exfil.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `http || tls`  (HTTP/TLS requests, hosts and user agents). Then Statistics > Conversations to see the talkers.
4. Same filter from the shell with tshark: `tshark -r http-post-exfil.pcap -Y "http || tls"`, and list talkers with `tshark -r http-post-exfil.pcap -q -z conv,ip`.
5. Confirm against the answer: Repeated large HTTP POST bodies to one external host

## MITRE ATT&CK
- T1041 Exfiltration Over C2 Channel
- T1048 Exfiltration Over Alternative Protocol

## Tools
Crafted HTTP POST exfil (scapy) over generated background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
