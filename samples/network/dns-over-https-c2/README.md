# dns-over-https-c2

## Scenario
A host (10.10.1.63) tunnels command and control over DNS-over-HTTPS. Instead of normal UDP 53, it POSTs application/dns-message bodies to a DoH resolver endpoint from a non browser process, hiding the DNS inside HTTPS.

## Timeline (UTC+3)
```
09:25:00  First DoH POST to /dns-query on 104.16.249.249
all day   POSTs about every 45s, ~450 of them
```

## How to spot it
- POST requests to /dns-query with Content-Type application/dns-message
- DNS is leaving over 443, not UDP 53, so normal DNS logging misses it
- A single internal host drives steady DoH from a non browser
- Regular timing points to automation, not human browsing

## Questions
```
1. Host?             10.10.1.63
2. Channel?          DNS over HTTPS (DoH)
3. Endpoint path?    /dns-query
4. Content type?     application/dns-message
5. Why it evades DNS logs?  DNS is inside HTTPS on 443
```

## MITRE ATT&CK
- T1071.004 Application Layer Protocol: DNS
- T1572 Protocol Tunneling

## Tools
Crafted DoH POST traffic (scapy) over generated background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
