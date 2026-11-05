# Tcp Session Hijacking

## Scenario

An attacker on the segment injected a spoofed TCP segment into an established Telnet session using the correct sequence and acknowledgement numbers, taking over the session to run commands.

Evidence: `tcp-session-hijacking.pcap`  
Capture date: 2022-01-27

## Timeline

~13:01  A client Telnet session is active; a segment with the client source but attacker MAC injects a command.

## How to spot it

- An injected segment with a spoofed source IP
- Correct seq/ack but a different source MAC
- Commands appear that the client did not type
- Cleartext Telnet makes seq prediction easy

## Questions

```
1. Protocol?          Telnet (TCP 23)
2. Tell?               spoofed IP, wrong MAC, valid seq
3. Injected?            cat /etc/shadow
4. Why possible?        cleartext, predictable seq
5. Technique?          TCP session hijack
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 18f28625f5397eab6e2a0ee732b8a9fc1ecbc248833b66c6f4de48bf30ea6c09.zip -phidden`. You get `tcp-session-hijacking.pcap`.
2. Open `tcp-session-hijacking.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `tcp.flags.syn==1 && tcp.flags.ack==0`  (SYN probes across ports). Then Statistics > Conversations to see the talkers.
4. Same filter from the shell with tshark: `tshark -r tcp-session-hijacking.pcap -Y "tcp.flags.syn==1 && tcp.flags.ack==0"`, and list talkers with `tshark -r tcp-session-hijacking.pcap -q -z conv,ip`.
5. Confirm against the answer: An injected segment with a spoofed source IP

## MITRE ATT&CK

- T1563 Remote Service Session Hijacking

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
