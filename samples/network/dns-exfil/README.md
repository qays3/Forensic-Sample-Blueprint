# dns-exfil

## Scenario
A workstation (10.10.1.71) is leaking data out through DNS. It sends a long run of TXT  
queries to subdomains of sync.datapipe-telemetry.net, where each label carries base32  
encoded data. The queries get NXDOMAIN style replies, which is normal for a tunnel.  

## Timeline (UTC+3)
```
13:10:00  Exfil starts from 10.10.1.71
13:1x:xx  58 TXT queries to *.datapipe-telemetry.net, one chunk each
```

## How to spot it
- Many DNS queries to subdomains of one odd domain in a short time  
- Each subdomain is long and random looking (encoded data), with a counter label  
- TXT record type and a domain no normal software uses  
- Decoding the labels (base32) rebuilds the stolen content, which starts with a marker  

## Questions
```
1. Exfil host?                  10.10.1.71
2. Attacker domain?             sync.datapipe-telemetry.net
3. Record type used?            TXT
4. How many chunks?             58
5. Encoding?                    base32 in the subdomain labels
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 70ba30eddd799aecab69ee0224722ee7a4d5e7742fe4baf2a02a682855818f93.zip -phidden`. You get `dns-exfil.pcap`.
2. Open `dns-exfil.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `dns`  (DNS queries and answers). Then Statistics > Conversations to see the talkers.
4. Same filter from the shell with tshark: `tshark -r dns-exfil.pcap -Y "dns"`, and list talkers with `tshark -r dns-exfil.pcap -q -z conv,ip`.
5. Confirm against the answer: Many DNS queries to subdomains of one odd domain in a short time

## MITRE ATT&CK
T1048.003 Exfiltration Over Alternative Protocol: Unencrypted Non C2  
T1071.004 Application Layer Protocol: DNS  

## Tools
Crafted DNS tunnel (scapy) over generated background traffic  
Packets: 6023  

## Signature
Forensic Sample Blueprint  |  qayssarayra.com
