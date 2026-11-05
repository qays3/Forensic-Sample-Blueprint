# Ssh Brute Force

## Scenario

An SSH brute force against 10.10.2.55: a high rate of short lived SSH connections from a handful of hosts, each dropped after the banner, the Mirai style login hammering.

Evidence: `ssh-brute-force.pcap`  
Capture date: 2023-12-21

## Timeline

~12:00  Hundreds of short SSH connections to 22 from a few sources, each cut after the banner.

## How to spot it

- Many short lived connections to SSH 22
- A handful of repeat source IPs
- Each session dropped quickly
- High rate of login attempts

## Questions

```
1. Protocol?          SSH (22)
2. Pattern?            many short connections
3. Sources?            a few repeat IPs
4. Goal?               guess credentials
5. ATT&CK?             T1110
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x <sha256>.zip -phidden`. You get `ssh-brute-force.pcap`.
2. Open `ssh-brute-force.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `tcp.port==22`  (the connection storm). Then Statistics > Conversations for the talkers.
4. Same filter from the shell: `tshark -r ssh-brute-force.pcap -Y "tcp.port==22"`, and `tshark -r ssh-brute-force.pcap -q -z conv,ip`.
5. Confirm against the answer: Many short lived connections to SSH 22

## MITRE ATT&CK

- T1110 Brute Force

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
