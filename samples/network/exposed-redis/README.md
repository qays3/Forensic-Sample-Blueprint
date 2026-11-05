# Exposed Redis

## Scenario

An exposed Redis instance on 10.10.2.80 is accessed without authentication from 10.10.1.66: unauthenticated PING, INFO and CONFIG commands against the open port.

Evidence: `exposed-redis.pcap`  
Capture date: 2020-10-05

## Timeline

~10:01  Unauthenticated Redis commands (PING, INFO, CONFIG GET/SET) against an open 6379.

## How to spot it

- Connection to Redis port 6379
- Commands run without any AUTH
- INFO and CONFIG reconnaissance
- No authentication required by the server

## Questions

```
1. Service?           Redis (6379)
2. Auth?               none required
3. Commands?            PING, INFO, CONFIG
4. Risk?               unauthenticated access
5. ATT&CK?             T1133
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x <sha256>.zip -phidden`. You get `exposed-redis.pcap`.
2. Open `exposed-redis.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `tcp.port==6379`  (the Redis commands). Then Statistics > Conversations for the talkers.
4. Same filter from the shell: `tshark -r exposed-redis.pcap -Y "tcp.port==6379"`, and `tshark -r exposed-redis.pcap -q -z conv,ip`.
5. Confirm against the answer: Connection to Redis port 6379

## MITRE ATT&CK

- T1133 External Remote Services

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
