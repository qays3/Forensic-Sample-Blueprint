# Cloud Storage Payload

## Scenario

A payload is pulled from a public cloud storage bucket: a host downloads a binary from a storage provider host, abusing trusted infrastructure for delivery.

Evidence: `cloud-storage-payload.pcap`  
Capture date: 2019-11-12

## Timeline

~11:03  HTTPS download of a binary from a cloud storage host.

## How to spot it

- Download from a cloud storage provider host
- A trusted looking domain hosting a binary payload
- Large opaque binary body
- Trusted infrastructure abused for delivery

## Questions

```
1. Technique?         Cloud storage payload
2. Host?               files.storage-host.example
3. Object?             stage2.bin
4. Why trusted?         known storage provider
5. ATT&CK?             T1105
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x <sha256>.zip -phidden`. You get `cloud-storage-payload.pcap`.
2. Open `cloud-storage-payload.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `tls || http`  (the storage download). Then Statistics > Conversations for the talkers.
4. Same filter from the shell: `tshark -r cloud-storage-payload.pcap -Y "tls || http"`, and `tshark -r cloud-storage-payload.pcap -q -z conv,ip`.
5. Confirm against the answer: Download from a cloud storage provider host

## MITRE ATT&CK

- T1105 Ingress Tool Transfer
- T1608 Stage Capabilities

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
