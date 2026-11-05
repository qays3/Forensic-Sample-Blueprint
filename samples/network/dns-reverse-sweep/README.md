# Dns Reverse Sweep

## Scenario

10.10.1.66 swept the 10.10.2.0/24 range with reverse (PTR) lookups to map internal hostnames.

Evidence: `dns-reverse-sweep.pcap`  
Capture date: 2019-06-05

## Timeline

~10:08  Sequential PTR queries for 10.10.2.1 through .39; some resolve to server names.

## How to spot it

- Many PTR queries walking an in-addr.arpa range
- Sequential host octets from one source
- Hostnames leak for live servers
- Reverse mapping of an internal subnet

## Questions

```
1. Scanner?           10.10.1.66
2. Query type?          PTR (reverse)
3. Range?               10.10.2.0/24
4. Goal?               Map internal hostnames
5. Technique?          Reverse lookup sweep
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x b145b7c83d4d9bb9cef7423f01ddc5ef27bf08004697dfb813b9d4f90489e242.zip -phidden`. You get `dns-reverse-sweep.pcap`.
2. Open `dns-reverse-sweep.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `dns`  (DNS queries and answers). Then Statistics > Conversations to see the talkers.
4. Same filter from the shell with tshark: `tshark -r dns-reverse-sweep.pcap -Y "dns"`, and list talkers with `tshark -r dns-reverse-sweep.pcap -q -z conv,ip`.
5. Confirm against the answer: Many PTR queries walking an in-addr.arpa range

## MITRE ATT&CK

- T1590 Gather Victim Network Information
- T1595 Active Scanning

## Tools

Wireshark, tshark, zeek, tcpdump. Filter by the scanner IP to isolate the activity.

## Signature

Forensic Sample Blueprint | qayssarayra.com
