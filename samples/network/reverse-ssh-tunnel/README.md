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

## MITRE ATT&CK
- T1572 Protocol Tunneling
- T1021.004 Remote Services: SSH

## Tools
Crafted reverse SSH tunnel (scapy) over generated background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
