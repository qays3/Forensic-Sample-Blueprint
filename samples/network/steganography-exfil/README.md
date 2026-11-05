# Steganography Exfil

## Scenario

Stolen data is hidden inside an image and uploaded: a PNG with extra payload bytes is POSTed to an image sharing host.

Evidence: `steganography-exfil.pcap`  
Capture date: 2021-08-17

## Timeline

~14:11  A PNG upload to img-share-corp.xyz larger than the image warrants.

## How to spot it

- An image upload bigger than the visible image
- PNG header then extra payload
- Upload to an image sharing host
- Data hidden in the file

## Questions

```
1. Carrier?           PNG image
2. Destination?         img-share-corp.xyz
3. Tell?               oversized image upload
4. Hidden?              payload after image data
5. Technique?          Exfil via steganography
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x <sha256>.zip -phidden`. You get `steganography-exfil.pcap`.
2. Open `steganography-exfil.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `http.request.method=="POST" && http.content_type contains "image"`  (The image upload). Then Statistics > Conversations for the talkers.
4. Same filter from the shell: `tshark -r steganography-exfil.pcap -Y "http.request.method=="POST" && http.content_type contains "image""`, and `tshark -r steganography-exfil.pcap -q -z conv,ip`.
5. Confirm against the answer: An image upload bigger than the visible image

## MITRE ATT&CK

- T1027.003 Steganography
- T1567 Exfiltration Over Web Service

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
