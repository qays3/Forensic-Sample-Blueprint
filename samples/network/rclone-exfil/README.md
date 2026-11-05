# Rclone Exfil

## Scenario

rclone uploads data to a cloud storage provider (Mega): a sustained TLS upload to the Mega API after a config was dropped.

Evidence: `rclone-exfil.pcap`  
Capture date: 2020-05-20

## Timeline

~14:03  A long TLS session to g.api.mega.co.nz pushing a large volume out.

## How to spot it

- Sustained TLS upload to a cloud storage host
- SNI of Mega, Dropbox or S3
- Large bytes out over one session
- rclone on the endpoint

## Questions

```
1. Tool?              rclone
2. Destination?         Mega (g.api.mega.co.nz)
3. Direction?           large upload
4. Tell?               cloud storage SNI + volume
5. Technique?          Exfil to cloud storage
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x <sha256>.zip -phidden`. You get `rclone-exfil.pcap`.
2. Open `rclone-exfil.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `tls.handshake.extensions_server_name contains "mega"`  (Upload to Mega). Then Statistics > Conversations for the talkers.
4. Same filter from the shell: `tshark -r rclone-exfil.pcap -Y "tls.handshake.extensions_server_name contains "mega""`, and `tshark -r rclone-exfil.pcap -q -z conv,ip`.
5. Confirm against the answer: Sustained TLS upload to a cloud storage host

## MITRE ATT&CK

- T1567.002 Exfiltration to Cloud Storage

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
