# Gmail Upload Exfil

## Scenario

An insider uploads company files to a personal Gmail: a large TLS upload to mail.google.com attaches the data to a personal account.

Evidence: `gmail-upload-exfil.pcap`  
Capture date: 2020-04-06

## Timeline

~14:13  A large upload to mail.google.com, attaching company files to personal mail.

## How to spot it

- A large upload to a personal webmail host
- TLS SNI of mail.google.com
- Big outbound on a work host
- Insider exfil channel

## Questions

```
1. Service?           personal Gmail
2. Action?             attachment upload
3. Direction?           large upload
4. Risk?               company files to personal mail
5. Technique?          Insider exfil
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x <sha256>.zip -phidden`. You get `gmail-upload-exfil.pcap`.
2. Open `gmail-upload-exfil.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `tls.handshake.extensions_server_name contains "google"`  (The upload to Gmail). Then Statistics > Conversations for the talkers.
4. Same filter from the shell: `tshark -r gmail-upload-exfil.pcap -Y "tls.handshake.extensions_server_name contains "google""`, and `tshark -r gmail-upload-exfil.pcap -q -z conv,ip`.
5. Confirm against the answer: A large upload to a personal webmail host

## MITRE ATT&CK

- T1567.002 Exfiltration to Cloud Storage

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
