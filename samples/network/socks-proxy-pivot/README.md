# socks-proxy-pivot

## Scenario
A compromised host (10.10.1.50) is used as a SOCKS proxy pivot. The attacker routes connections through it to reach many internal servers on many ports, so one foothold scans and reaches the whole internal range.

## Timeline (UTC+3)
```
09:55:00  10.10.1.50 starts reaching many internal hosts and ports
09:5x:xx  Connections to 10.10.2.0/24 on 445, 3389, 22, 1433, 5985
```

## How to spot it
- One internal host suddenly connects to many others on many ports
- The spread of ports and hosts looks like a scan from inside
- This host is the pivot, the real operator is behind it
- Normal workstations do not fan out to servers like this

## Questions
```
1. Pivot host?       10.10.1.50
2. What is it doing?  Proxying/pivoting to internal hosts
3. Ports reached?    445, 3389, 22, 1433, 5985
4. Range hit?        10.10.2.0/24
5. Why one host?     SOCKS proxy through the foothold
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 811199fa32917636992b19747c28eaa012ca7a522533ff194e39fd2fa9aa1992.zip -phidden`. You get `socks-proxy-pivot.pcap`.
2. Open `socks-proxy-pivot.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `http || tls`  (HTTP/TLS requests, hosts and user agents). Then Statistics > Conversations to see the talkers.
4. Same filter from the shell with tshark: `tshark -r socks-proxy-pivot.pcap -Y "http || tls"`, and list talkers with `tshark -r socks-proxy-pivot.pcap -q -z conv,ip`.
5. Confirm against the answer: One internal host suddenly connects to many others on many ports

## MITRE ATT&CK
- T1090.001 Proxy: Internal Proxy
- T1046 Network Service Discovery

## Tools
Crafted SOCKS pivot traffic (scapy) over generated background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
