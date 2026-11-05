# Cryptominer Stratum

## Scenario

A host joins a mining pool over the stratum protocol: a login with a wallet and worker, then a steady stream of share submissions.

Evidence: `cryptominer-stratum.pcap`  
Capture date: 2019-04-28

## Timeline

~12:05  Stratum login to a mining pool on 3333, then regular share submits.

## How to spot it

- Stratum JSON login with a wallet address
- xmrig user agent
- Connection to a mining pool port (3333)
- Steady share submissions

## Questions

```
1. Protocol?          Stratum (mining)
2. Pool?               51.75.78.12:3333
3. Tell?               login with wallet, xmrig
4. Pattern?            steady submits
5. Technique?          Resource hijacking
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x <sha256>.zip -phidden`. You get `cryptominer-stratum.pcap`.
2. Open `cryptominer-stratum.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `tcp.port==3333`  (the stratum traffic). Then Statistics > Conversations for the talkers.
4. Same filter from the shell: `tshark -r cryptominer-stratum.pcap -Y "tcp.port==3333"`, and `tshark -r cryptominer-stratum.pcap -q -z conv,ip`.
5. Confirm against the answer: Stratum JSON login with a wallet address

## MITRE ATT&CK

- T1496 Resource Hijacking

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
