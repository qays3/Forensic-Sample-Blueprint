# Empire Http C2

## Scenario

PowerShell Empire beaconing over HTTP to 45.77.65.211 with base64 data in a session cookie on a fixed interval.

Evidence: `empire-http-c2.pcap`  
Capture date: 2025-01-04

## Timeline

~13:00  Regular GET /news.php with a base64 session cookie to a fake news CDN.

## How to spot it

- Regular HTTP GET with a base64 cookie
- Fixed beacon interval
- A news or blog style URI
- Encoded task in the response

## Questions

```
1. C2?                45.77.65.211
2. Channel?            HTTP cookie
3. Encoding?            base64 session cookie
4. Tool?               PowerShell Empire
5. Technique?          Web C2
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x <sha256>.zip -phidden`. You get `empire-http-c2.pcap`.
2. Open `empire-http-c2.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `http.cookie contains "session"`  (Empire cookie beacons). Then Statistics > Conversations for the talkers.
4. Same filter from the shell: `tshark -r empire-http-c2.pcap -Y "http.cookie contains "session""`, and `tshark -r empire-http-c2.pcap -q -z conv,ip`.
5. Confirm against the answer: Regular HTTP GET with a base64 cookie

## MITRE ATT&CK

- T1071.001 Web Protocols
- T1132.001 Standard Encoding

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
