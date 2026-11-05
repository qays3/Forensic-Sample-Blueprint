# Cryptominer Dropper

## Scenario

A Linux host fetches a miner install script over HTTP (the curl pipe sh pattern) and then connects to a mining pool.

Evidence: `cryptominer-dropper.pcap`  
Capture date: 2023-03-18

## Timeline

~10:03  HTTP GET for install.sh, then a stratum login to a mining pool.

## How to spot it

- HTTP GET for a shell install script
- Script fetches and runs a miner
- Followed by a stratum pool connection
- curl pipe sh style delivery

## Questions

```
1. Delivery?          curl pipe sh
2. Script?             /install.sh
3. Then?               stratum pool 3333
4. Payload?            cryptominer
5. ATT&CK?             T1496
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x <sha256>.zip -phidden`. You get `cryptominer-dropper.pcap`.
2. Open `cryptominer-dropper.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `http.request || tcp.port==3333`  (the script fetch and the pool connection). Then Statistics > Conversations for the talkers.
4. Same filter from the shell: `tshark -r cryptominer-dropper.pcap -Y "http.request || tcp.port==3333"`, and `tshark -r cryptominer-dropper.pcap -q -z conv,ip`.
5. Confirm against the answer: HTTP GET for a shell install script

## MITRE ATT&CK

- T1105 Ingress Tool Transfer
- T1496 Resource Hijacking

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
