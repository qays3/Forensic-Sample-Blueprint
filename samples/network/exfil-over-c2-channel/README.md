# Exfil Over C2 Channel

## Scenario

A large upload rides the existing C2 channel: a big HTTP POST to the beacon host moves stolen data out.

Evidence: `exfil-over-c2-channel.pcap`  
Capture date: 2023-04-09

## Timeline

~14:01  A multi megabyte POST to the C2 host, far bigger than the usual beacons.

## How to spot it

- A large POST to the C2 host
- Upload far bigger than normal beacons
- Same destination as the beacon
- Bytes out spike

## Questions

```
1. Channel?           existing C2 (POST)
2. Size?               multi MB upload
3. Destination?         the beacon host
4. Tell?               big outbound POST
5. Technique?          Exfil over C2
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x <sha256>.zip -phidden`. You get `exfil-over-c2-channel.pcap`.
2. Open `exfil-over-c2-channel.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `http.request.method=="POST" && ip.addr==185.220.101.9`  (The large POST). Then Statistics > Conversations for the talkers.
4. Same filter from the shell: `tshark -r exfil-over-c2-channel.pcap -Y "http.request.method=="POST" && ip.addr==185.220.101.9"`, and `tshark -r exfil-over-c2-channel.pcap -q -z conv,ip`.
5. Confirm against the answer: A large POST to the C2 host

## MITRE ATT&CK

- T1041 Exfiltration Over C2 Channel

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
