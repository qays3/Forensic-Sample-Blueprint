# Github Exfil

## Scenario

Data is pushed to a private GitHub repo: a git push over HTTPS to github.com moves the stolen files out under cover of a normal developer service.

Evidence: `github-exfil.pcap`  
Capture date: 2024-03-24

## Timeline

~14:07  A git receive-pack push to a new repo on github.com with a large upload.

## How to spot it

- git push (receive-pack) to github over TLS
- A new or unknown repo
- Large upload to a dev service
- Blends with normal developer traffic

## Questions

```
1. Service?           GitHub
2. Operation?           git push (receive-pack)
3. Repo?               attacker/stolen
4. Tell?               large upload to a code host
5. Technique?          Exfil to code repo
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x <sha256>.zip -phidden`. You get `github-exfil.pcap`.
2. Open `github-exfil.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `tls.handshake.extensions_server_name contains "github"`  (The push to GitHub). Then Statistics > Conversations for the talkers.
4. Same filter from the shell: `tshark -r github-exfil.pcap -Y "tls.handshake.extensions_server_name contains "github""`, and `tshark -r github-exfil.pcap -q -z conv,ip`.
5. Confirm against the answer: git push (receive-pack) to github over TLS

## MITRE ATT&CK

- T1567 Exfiltration Over Web Service

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
