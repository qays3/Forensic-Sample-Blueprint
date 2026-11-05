# Voip Sip Flood

## Scenario

A host flooded the PBX with SIP INVITE requests and tried REGISTER guesses, a VoIP denial of service combined with toll fraud reconnaissance.

Evidence: `voip-sip-flood.pcap`  
Capture date: 2025-10-25

## Timeline

~12:11  A rapid burst of SIP INVITEs to UDP 5060, then REGISTER attempts against extensions.

## How to spot it

- A high rate of SIP INVITE to UDP 5060
- One source hitting many called numbers
- REGISTER attempts guessing extensions
- PBX overwhelmed / fraud attempts

## Questions

```
1. Protocol?          SIP (UDP 5060)
2. Flood?              INVITE burst
3. Plus?               REGISTER guessing
4. Goals?               DoS + toll fraud
5. Technique?          VoIP flood
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 5ffb2f88940fc5e8178a49b0da6d02dcac6d2a4242cb1a920d35f66d8925c2fa.zip -phidden`. You get `voip-sip-flood.pcap`.
2. Open `voip-sip-flood.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `sip`  (SIP INVITE/REGISTER messages). Then Statistics > Conversations to see the talkers.
4. Same filter from the shell with tshark: `tshark -r voip-sip-flood.pcap -Y "sip"`, and list talkers with `tshark -r voip-sip-flood.pcap -q -z conv,ip`.
5. Confirm against the answer: A high rate of SIP INVITE to UDP 5060

## MITRE ATT&CK

- T1498 Network Denial of Service

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
