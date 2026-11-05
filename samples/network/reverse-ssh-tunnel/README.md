# reverse-ssh-tunnel

## Scenario
A host (10.10.1.73) builds a reverse SSH tunnel to the attacker at 85.202.169.44 on port 443. SSH on 443 slips past firewalls that allow web traffic, and the long lived session gives the attacker a way back in.

## Timeline (UTC+3)
```
09:50:00  Reverse SSH tunnel from 10.10.1.73 to 85.202.169.44:443
09:50:xx  Long lived encrypted session (SSH banner on a web port)
```

## How to spot it
- SSH protocol banner on port 443 instead of 22
- A single long lived outbound session to an external host
- SSH on a web port is a firewall evasion trick
- The internal host initiates and keeps the tunnel open

## Questions
```
1. Host?             10.10.1.73
2. Attacker and port?  85.202.169.44:443
3. Protocol on 443?  SSH (not TLS)
4. Why port 443?     Slip past web only firewall rules
5. Session type?     Long lived reverse tunnel
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x ef2f2314e9e45a72c93ae597ffce9a0f64d8cb01600c30dd3971d74cef9da9b5.zip -phidden`. You get `reverse-ssh-tunnel.pcap`.
2. Open `reverse-ssh-tunnel.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `dns`  (DNS queries and answers). Then Statistics > Conversations to see the talkers.
4. Same filter from the shell with tshark: `tshark -r reverse-ssh-tunnel.pcap -Y "dns"`, and list talkers with `tshark -r reverse-ssh-tunnel.pcap -q -z conv,ip`.
5. Confirm against the answer: SSH protocol banner on port 443 instead of 22

## MITRE ATT&CK
- T1572 Protocol Tunneling
- T1021.004 Remote Services: SSH

## Tools
Crafted reverse SSH tunnel (scapy) over generated background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
