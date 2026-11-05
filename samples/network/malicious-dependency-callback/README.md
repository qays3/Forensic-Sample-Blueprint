# Malicious Dependency Callback

## Scenario

A malicious dependency runs on a build server during install and beacons the CI environment details back to the attacker.

Evidence: `malicious-dependency-callback.pcap`  
Capture date: 2025-04-03

## Timeline

~11:00  An install time HTTP callback carrying CI and host details to an external endpoint.

## How to spot it

- An install time callback from a build or CI host
- Environment and host info in the request
- An unexpected outbound connection during a build
- Dependency executing code at install

## Questions

```
1. Technique?         Malicious dependency
2. Source?             a build / CI host
3. Callback?           dep-telemetry.example
4. Data?               CI environment details
5. ATT&CK?             T1195.001
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x <sha256>.zip -phidden`. You get `malicious-dependency-callback.pcap`.
2. Open `malicious-dependency-callback.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `http.request`  (the install callback). Then Statistics > Conversations for the talkers.
4. Same filter from the shell: `tshark -r malicious-dependency-callback.pcap -Y "http.request"`, and `tshark -r malicious-dependency-callback.pcap -q -z conv,ip`.
5. Confirm against the answer: An install time callback from a build or CI host

## MITRE ATT&CK

- T1195.001 Compromise Software Dependencies

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
