# smb-named-pipe-c2

## Scenario
Two internal hosts (10.10.1.50 and 10.10.1.51) talk over an SMB named pipe for peer to peer command and control. One beacon relays through the other over \PIPE\msagent_*, a Cobalt Strike style pipe, so only one host needs to reach the internet.

## Timeline (UTC+3)
```
09:36:40  SMB named pipe sessions begin between 10.10.1.50 and 10.10.1.51
09:3x:xx  Repeated \PIPE\msagent_* traffic
```

## How to spot it
- Internal host to host SMB with an odd named pipe (msagent_*)
- No external traffic from the second host, it is chained
- Named pipe C2 is used to pivot deeper in a network
- Pipe names look random but follow a pattern

## Questions
```
1. The two hosts?    10.10.1.50 and 10.10.1.51
2. Channel?          SMB named pipe
3. Pipe pattern?     \PIPE\msagent_*
4. Why used?         Peer to peer pivot, one host reaches internet
5. Protocol/port?    SMB over 445
```

## MITRE ATT&CK
- T1071 Application Layer Protocol
- T1090.001 Internal Proxy

## Tools
Crafted SMB named pipe C2 (scapy) over generated background traffic

## Signature
Forensic Sample Blueprint | qayssarayra.com
