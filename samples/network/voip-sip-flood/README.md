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

## MITRE ATT&CK

- T1498 Network Denial of Service

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
