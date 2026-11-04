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

## MITRE ATT&CK
T1071.001 Application Layer Protocol: Web Protocols  
T1573 Encrypted Channel  
T1008 Fallback Channels (jitter)  

## Tools
Crafted beacon traffic (scapy) over generated background traffic  
Packets: 10481  

## Signature
Forensic Sample Blueprint  |  qayssarayra.com
