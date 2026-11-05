# Compromised Site Staging

## Scenario

Malware is staged on a compromised legitimate website: a host downloads an executable from an uploads folder on an otherwise normal looking site.

Evidence: `compromised-site-staging.pcap`  
Capture date: 2024-05-07

## Timeline

~10:01  HTTP GET for an .exe from the uploads folder of a legit looking site.

## How to spot it

- Payload hosted under a legit site path (wp-content/uploads)
- An executable with an MZ header
- The site itself looks legitimate
- Attacker using a hacked host for delivery

## Questions

```
1. Technique?         Compromised site staging
2. Host?               legit-news-site.example
3. Path?               wp-content/uploads
4. Payload?            doc.exe (MZ)
5. ATT&CK?             T1608.001
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x <sha256>.zip -phidden`. You get `compromised-site-staging.pcap`.
2. Open `compromised-site-staging.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `http.request.uri contains ".exe"`  (the payload download). Then Statistics > Conversations for the talkers.
4. Same filter from the shell: `tshark -r compromised-site-staging.pcap -Y "http.request.uri contains ".exe""`, and `tshark -r compromised-site-staging.pcap -q -z conv,ip`.
5. Confirm against the answer: Payload hosted under a legit site path (wp-content/uploads)

## MITRE ATT&CK

- T1608.001 Upload Malware
- T1105 Ingress Tool Transfer

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
