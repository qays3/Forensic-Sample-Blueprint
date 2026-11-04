# cobalt-strike-like-profile

## Scenario
A host (10.10.1.52) beacons to a Cobalt Strike style team server (45.153.240.11) through the domain static-cdn-assets.com. It uses a malleable profile: a legit looking GET, an outdated IE user agent, and a base64 cookie that carries the beacon metadata. Check in is about every minute with small jitter.

## Timeline (UTC+3)
```
09:20:00  First beacon to static-cdn-assets.com (45.153.240.11)
all day   GET requests about every 60s with a base64 SESSIONID cookie, ~461 check ins
```

## How to spot it
- Regular interval GET requests to one host, the malleable profile
- Outdated IE user agent that no real browser here uses
- Beacon metadata hidden in a base64 cookie
- URIs mimic analytics (ga.js, __utm.gif) but one host drives them

## Questions
```
1. Beaconing host?   10.10.1.52
2. Team server?      45.153.240.11 (static-cdn-assets.com)
3. Interval?         ~60s with jitter
4. Where is metadata?  base64 SESSIONID cookie
5. User agent tell?  Old IE/Trident string
```

## MITRE ATT&CK
- T1071.001 Application Layer Protocol: Web Protocols
- T1071.004 not used
- T1105 Ingress Tool Transfer

## Tools
Crafted Cobalt Strike style HTTP beacon (scapy) over generated background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
