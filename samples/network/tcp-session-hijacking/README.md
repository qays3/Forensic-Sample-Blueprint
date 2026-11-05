# Tcp Session Hijacking

## Scenario

An attacker on the segment injected a spoofed TCP segment into an established Telnet session using the correct sequence and acknowledgement numbers, taking over the session to run commands.

Evidence: `tcp-session-hijacking.pcap`  
Capture date: 2022-01-27

## Timeline

~13:01  A client Telnet session is active; a segment with the client source but attacker MAC injects a command.

## How to spot it

- An injected segment with a spoofed source IP
- Correct seq/ack but a different source MAC
- Commands appear that the client did not type
- Cleartext Telnet makes seq prediction easy

## Questions

```
1. Protocol?          Telnet (TCP 23)
2. Tell?               spoofed IP, wrong MAC, valid seq
3. Injected?            cat /etc/shadow
4. Why possible?        cleartext, predictable seq
5. Technique?          TCP session hijack
```

## MITRE ATT&CK

- T1563 Remote Service Session Hijacking

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
