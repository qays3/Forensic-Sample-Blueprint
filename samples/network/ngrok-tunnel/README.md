# ngrok-tunnel

## Scenario
A host (10.10.1.88) opens an ngrok tunnel, connecting to an *.ngrok.io endpoint over TLS. ngrok exposes internal services to the internet or gives an attacker a stable tunnel, bypassing inbound firewall rules.

## Timeline (UTC+3)
```
10:05:00  10.10.1.88 resolves tunnel.ngrok.io and opens a TLS tunnel
10:05:xx  Long lived session to the ngrok edge (3.134.125.175)
```

## How to spot it
- DNS lookup and TLS SNI for an ngrok.io domain
- A long lived tunnel to the ngrok edge infrastructure
- Tunneling services bypass inbound firewall controls
- Unexpected on a managed corporate host

## Questions
```
1. Host?             10.10.1.88
2. Service?          ngrok tunnel
3. Domain?           tunnel.ngrok.io
4. Port?             443 (TLS)
5. Risk?             Exposes internal services or stable attacker tunnel
```

## MITRE ATT&CK
- T1572 Protocol Tunneling
- T1090 Proxy

## Tools
Crafted ngrok tunnel (scapy) over generated background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
