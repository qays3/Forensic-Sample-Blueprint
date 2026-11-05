# exfil-to-paste-site

## Scenario
A host (10.10.1.62) posts stolen data to a public paste site (pastebin.com). The attacker later reads it from the raw link, so the data leaves through a trusted, common service.

## Timeline (UTC+3)
```
14:00:00  10.10.1.62 POSTs to pastebin.com /api/api_post.php
14:00:xx  Response returns a raw paste URL
```

## How to spot it
- A POST to a paste site API from a workstation
- Data leaves through a trusted public service
- The response hands back a paste URL the attacker will read
- Uncommon for business workflows

## Questions
```
1. Host?             10.10.1.62
2. Service?          pastebin.com
3. Endpoint?         /api/api_post.php
4. What returns?     A raw paste URL
5. Why used?         Trusted service hides the exfil
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 69eeb87ea8b3e735a2036f73850ab7e75b1e7bc566607313df0510d4ed3d29b7.zip -phidden`. You get `exfil-to-paste-site.pcap`.
2. Open `exfil-to-paste-site.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `http || tls`  (HTTP/TLS requests, hosts and user agents). Then Statistics > Conversations to see the talkers.
4. Same filter from the shell with tshark: `tshark -r exfil-to-paste-site.pcap -Y "http || tls"`, and list talkers with `tshark -r exfil-to-paste-site.pcap -q -z conv,ip`.
5. Confirm against the answer: A POST to a paste site API from a workstation

## MITRE ATT&CK
- T1567.001 Exfiltration to Code Repository / Paste Site
- T1048 Exfiltration Over Alternative Protocol

## Tools
Crafted paste site exfil (scapy) over generated background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
