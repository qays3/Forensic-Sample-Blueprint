# Bind Shell

## Scenario

A bind shell on 10.10.2.40: the attacker 10.10.1.66 connects to a listening port and receives an interactive command prompt, with commands and their output visible in cleartext.

Evidence: `bind-shell.pcap`  
Capture date: 2024-07-23

## Timeline

~11:00  A connection to a high listening port returns a Windows command banner, then commands and output.

## How to spot it

- A command shell banner over a raw TCP port
- Cleartext commands like whoami and ipconfig
- Interactive request and response on a non standard port
- Attacker connecting inbound to a listener

## Questions

```
1. Technique?         Bind shell
2. Port?               4444
3. Direction?          attacker connects in
4. Tell?               cmd banner in cleartext
5. ATT&CK?             T1059
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x <sha256>.zip -phidden`. You get `bind-shell.pcap`.
2. Open `bind-shell.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `tcp.port==4444`  (the shell banner and commands). Then Statistics > Conversations for the talkers.
4. Same filter from the shell: `tshark -r bind-shell.pcap -Y "tcp.port==4444"`, and `tshark -r bind-shell.pcap -q -z conv,ip`.
5. Confirm against the answer: A command shell banner over a raw TCP port

## MITRE ATT&CK

- T1059 Command and Scripting Interpreter

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
