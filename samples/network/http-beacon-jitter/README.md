# http-beacon-jitter

## Scenario
A workstation (10.10.1.54) is beaconing to a command and control server at 162.33.14.90,  
which it reaches through the CDN looking domain cdn-edge-static.com. It checks in about  
every 60 seconds with random jitter, using a normal browser user agent, so it blends into  
web traffic.  

## Timeline (UTC+3)
```
09:20:00  First beacon from 10.10.1.54 to cdn-edge-static.com (162.33.14.90)
all day   Beacon repeats roughly every 60s (+/- 18s), about 459 check ins
```

## How to spot it
- Many connections to one host at a near regular interval, the jitter hides the exact period  
- Every request is tiny and the response is a fixed small size  
- The domain looks like a CDN but is young and only this one host talks to it  
- URIs and a session cookie look normal but never change behaviour  

## Questions
```
1. Infected host?               10.10.1.54
2. C2 IP and domain?            162.33.14.90, cdn-edge-static.com
3. Beacon interval?             about 60 seconds with jitter
4. How many check ins?          about 459
5. Why is it hard to see?        Normal user agent, CDN like name, hides among web traffic
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x c4d6dbbf195a3c3ce505c7cab37a232e90f7941d53a8a77d64b3b3828441dc72.zip -phidden`. You get `http-beacon-jitter.pcap`.
2. Open `http-beacon-jitter.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `http || tls`  (HTTP/TLS requests, hosts and user agents). Then Statistics > Conversations to see the talkers.
4. Same filter from the shell with tshark: `tshark -r http-beacon-jitter.pcap -Y "http || tls"`, and list talkers with `tshark -r http-beacon-jitter.pcap -q -z conv,ip`.
5. Confirm against the answer: Many connections to one host at a near regular interval, the jitter hides the exact period

## MITRE ATT&CK
T1071.001 Application Layer Protocol: Web Protocols  
T1573 Encrypted Channel  
T1008 Fallback Channels (jitter)  

## Tools
Crafted beacon traffic (scapy) over generated background traffic  
Packets: 10481  

## Signature
Forensic Sample Blueprint  |  qayssarayra.com
