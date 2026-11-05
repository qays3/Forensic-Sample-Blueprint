# masscan-high-rate

## Scenario
10.10.1.66 ran a masscan style high rate scan across 10.10.0.0/16, firing thousands of SYN  
packets per second from a fixed source port.  

## How to spot it
- Extremely high packet rate from one host  
- Fixed source port (61000) and fixed window (1024), hallmarks of masscan  
- SYN packets sprayed across many hosts and a few common ports  

## Questions
```
1. Scanner IP?        10.10.1.66
2. Tool style?        masscan (high rate SYN)
3. Fixed source port? 61000
4. Tell from nmap?    Much higher rate, fixed source port
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 88eaad19cb15a68df453d3d8b9f44908d0e492685edec60fa65b67c3e3806eb0.zip -phidden`. You get `masscan-high-rate.pcap`.
2. Open `masscan-high-rate.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `tcp.flags.syn==1 && tcp.flags.ack==0`  (SYN probes across ports). Then Statistics > Conversations to see the talkers.
4. Same filter from the shell with tshark: `tshark -r masscan-high-rate.pcap -Y "tcp.flags.syn==1 && tcp.flags.ack==0"`, and list talkers with `tshark -r masscan-high-rate.pcap -q -z conv,ip`.
5. Confirm against the answer: Extremely high packet rate from one host

## MITRE ATT&CK
T1595.001 Active Scanning: Scanning IP Blocks  

## Signature
Forensic Sample Blueprint  |  qayssarayra.com
