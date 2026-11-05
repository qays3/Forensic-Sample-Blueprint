# Exposed Docker Api

## Scenario

An exposed Docker API on 10.10.2.81:2375 is accessed from 10.10.1.66: an unauthenticated container is created with the host filesystem bind mounted, a container breakout path.

Evidence: `exposed-docker-api.pcap`  
Capture date: 2024-05-22

## Timeline

~11:00  Unauthenticated Docker API calls: version, create a container binding the host root, start it.

## How to spot it

- HTTP to the Docker API on 2375
- No TLS and no authentication
- A container created with / bind mounted
- Host filesystem exposed to the container

## Questions

```
1. Service?           Docker API (2375)
2. Auth?               none (plain HTTP)
3. Abuse?               bind mount host root
4. Result?             host access
5. ATT&CK?             T1610
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x <sha256>.zip -phidden`. You get `exposed-docker-api.pcap`.
2. Open `exposed-docker-api.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `tcp.port==2375`  (the Docker API calls). Then Statistics > Conversations for the talkers.
4. Same filter from the shell: `tshark -r exposed-docker-api.pcap -Y "tcp.port==2375"`, and `tshark -r exposed-docker-api.pcap -q -z conv,ip`.
5. Confirm against the answer: HTTP to the Docker API on 2375

## MITRE ATT&CK

- T1610 Deploy Container
- T1133 External Remote Services

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
