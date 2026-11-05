# udp-scan

## Scenario
10.10.1.66 ran a UDP scan of ports 1-499 on 10.10.2.15.  

## How to spot it
- UDP probes to many ports from one host  
- Closed UDP ports answer ICMP type 3 code 3 (port unreachable)  
- Ports with no reply may be open or filtered (here 53, 123, 161 stay silent)  

## Questions
```
1. Scanner IP?         10.10.1.66
2. Protocol?           UDP
3. Closed port sign?   ICMP port unreachable (type 3 code 3)
4. Likely open?        53, 123, 161 (no reply)
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x c608c3f1088cecc7c7784534f8e076b0032bac56bd84fb8396fff244299780d7.zip -phidden`. You get `udp-scan.pcap`.
2. Open `udp-scan.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `icmp.type==3`  (ICMP port-unreachable replies to UDP probes). Then Statistics > Conversations to see the talkers.
4. Same filter from the shell with tshark: `tshark -r udp-scan.pcap -Y "icmp.type==3"`, and list talkers with `tshark -r udp-scan.pcap -q -z conv,ip`.
5. Confirm against the answer: UDP probes to many ports from one host

## MITRE ATT&CK
T1046 Network Service Discovery  

## Signature
Forensic Sample Blueprint  |  qayssarayra.com
