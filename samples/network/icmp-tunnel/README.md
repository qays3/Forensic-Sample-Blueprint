# icmp-tunnel

## Scenario
A host (10.10.1.80) tunnels data to 91.219.236.17 inside ICMP echo packets. The ping payloads are large and carry encoded data, and the external host replies with matching echo replies, a covert channel that normal firewalls often allow.

## Timeline (UTC+3)
```
09:33:20  ICMP tunnel starts from 10.10.1.80 to 91.219.236.17
09:3x:xx  300 echo request/reply pairs with large, data carrying payloads
```

## How to spot it
- ICMP echo packets with large, non standard payloads
- A steady stream of ping to one external host
- Fixed ICMP id with incrementing sequence, carrying data both ways
- Normal ping is small and infrequent, this is bulk

## Questions
```
1. Host?             10.10.1.80
2. External peer?    91.219.236.17
3. Protocol abused?  ICMP echo (ping)
4. Tell from normal ping?  Large data payloads, continuous stream
5. ICMP id used?     1337
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x da083b98f5ee962723dbf96b328df589ac0e19074a416a5460e4101339400755.zip -phidden`. You get `icmp-tunnel.pcap`.
2. Open `icmp-tunnel.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `icmp`  (ICMP echo traffic). Then Statistics > Conversations to see the talkers.
4. Same filter from the shell with tshark: `tshark -r icmp-tunnel.pcap -Y "icmp"`, and list talkers with `tshark -r icmp-tunnel.pcap -q -z conv,ip`.
5. Confirm against the answer: ICMP echo packets with large, non standard payloads

## MITRE ATT&CK
- T1095 Non-Application Layer Protocol
- T1048.003 Exfiltration Over Alternative Protocol

## Tools
Crafted ICMP tunnel (scapy) over generated background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
