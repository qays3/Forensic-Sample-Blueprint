# tor-traffic

## Scenario
A host (10.10.1.84) connects to the Tor network, reaching known Tor guard relays on port 9001 with TLS. Tor use from a workstation can hide C2 or data theft behind onion routing.

## Timeline (UTC+3)
```
10:00:00  10.10.1.84 connects to Tor guard relays on 9001
10:0x:xx  Repeated TLS sessions to known Tor node IPs
```

## How to spot it
- Connections to IPs on public Tor relay lists
- Port 9001 and Tor style TLS certificates
- A workstation using Tor is unusual on a corporate network
- Destinations rotate across known guard nodes

## Questions
```
1. Host?             10.10.1.84
2. Network?          Tor
3. Port?             9001 (Tor OR port)
4. How to confirm?   Match IPs to the public Tor relay list
5. Why suspicious?   Hides C2 or exfil behind onion routing
```

## MITRE ATT&CK
- T1090.003 Proxy: Multi-hop Proxy

## Tools
Crafted Tor guard traffic (scapy) over generated background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
