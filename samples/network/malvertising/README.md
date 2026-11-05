# Malvertising

## Scenario

A malvertising chain: a malicious ad redirects the browser through a tracking hop to a fake download host that serves a payload.

Evidence: `malvertising.pcap`  
Capture date: 2022-01-08

## Timeline

~12:00  An ad request redirects through a tracker to a download host serving an executable.

## How to spot it

- A request to an ad network host
- A redirect chain through a click tracker
- Ending at a fake download host
- An executable delivered as a fake setup

## Questions

```
1. Technique?         Malvertising
2. Chain?              adserve-net, click-track, dl-secure
3. Payload?            setup.exe
4. Entry?              malicious ad
5. ATT&CK?             T1189
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x <sha256>.zip -phidden`. You get `malvertising.pcap`.
2. Open `malvertising.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `http`  (the ad redirect chain). Then Statistics > Conversations for the talkers.
4. Same filter from the shell: `tshark -r malvertising.pcap -Y "http"`, and `tshark -r malvertising.pcap -q -z conv,ip`.
5. Confirm against the answer: A request to an ad network host

## MITRE ATT&CK

- T1189 Drive-by Compromise
- T1608.006 SEO Poisoning

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
