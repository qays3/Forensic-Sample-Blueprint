# os-fingerprinting

## Scenario
10.10.1.66 sent crafted TCP probes with unusual TTL, window size and option orders to  
10.10.2.15 to fingerprint the operating system.  

## How to spot it
- A few hand crafted SYN packets with odd TTL (64, 128, 255), window and TCP options  
- The attacker reads the replies to guess the OS  
- Very low packet count compared with a port scan  

## Questions
```
1. Scanner IP?        10.10.1.66
2. Technique?         OS fingerprinting from TTL, window, TCP options
3. Target reply TTL?  64 (Linux like)
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x a8ffb6d085a3df30f4d9261f7bc8f63c523d7767b0e3620fb8ef8ec5a0d57c40.zip -phidden`. You get `os-fingerprinting.pcap`.
2. Open `os-fingerprinting.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `tcp.flags.syn==1 && tcp.flags.ack==0`  (SYN probes across ports). Then Statistics > Conversations to see the talkers.
4. Same filter from the shell with tshark: `tshark -r os-fingerprinting.pcap -Y "tcp.flags.syn==1 && tcp.flags.ack==0"`, and list talkers with `tshark -r os-fingerprinting.pcap -q -z conv,ip`.
5. Confirm against the answer: A few hand crafted SYN packets with odd TTL (64, 128, 255), window and TCP options

## MITRE ATT&CK
T1592.002 Gather Victim Host Information: Software  

## Signature
Forensic Sample Blueprint  |  qayssarayra.com
