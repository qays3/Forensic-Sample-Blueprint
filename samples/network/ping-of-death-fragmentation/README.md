# Ping Of Death Fragmentation

## Scenario

An attacker sent a stream of ICMP fragments that reassemble to more than 65535 bytes, an oversized ping (ping of death) designed to crash or hang a vulnerable stack.

Evidence: `ping-of-death-fragmentation.pcap`  
Capture date: 2022-03-26

## Timeline

~11:05  Many MF ICMP fragments with the same IP id, reassembling beyond the legal maximum.

## How to spot it

- Many fragments sharing one IP id
- More fragments (MF) flag set throughout
- Reassembled size exceeds 65535 bytes
- High fragment offset at the tail

## Questions

```
1. Protocol?          ICMP (fragmented)
2. Tell?               Reassembly > 65535 bytes
3. IP id?              shared across fragments
4. Goal?               Crash/hang the target
5. Technique?          Ping of death
```

## MITRE ATT&CK

- T1499.001 OS Exhaustion Flood

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
