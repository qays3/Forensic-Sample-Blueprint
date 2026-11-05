# Malicious Package Fetch

## Scenario

A malicious open source package is installed: the registry tarball is fetched, then a postinstall script beacons host details to an attacker endpoint.

Evidence: `malicious-package-fetch.pcap`  
Capture date: 2021-01-06

## Timeline

~10:03  Package tarball fetched from the registry, then an install time callback to an external host.

## How to spot it

- A package tarball fetched from a registry
- A callback right after install
- Host and user details in the callback URL
- Postinstall script phoning home

## Questions

```
1. Technique?         Malicious package
2. Package?             leftpad-utils 9.9.9
3. Callback?           telemetry-collect.example
4. Trigger?            postinstall script
5. ATT&CK?             T1195.001
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x <sha256>.zip -phidden`. You get `malicious-package-fetch.pcap`.
2. Open `malicious-package-fetch.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `http || tls`  (the tarball fetch and the callback). Then Statistics > Conversations for the talkers.
4. Same filter from the shell: `tshark -r malicious-package-fetch.pcap -Y "http || tls"`, and `tshark -r malicious-package-fetch.pcap -q -z conv,ip`.
5. Confirm against the answer: A package tarball fetched from a registry

## MITRE ATT&CK

- T1195.001 Compromise Software Dependencies
- T1059 Command and Scripting

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
