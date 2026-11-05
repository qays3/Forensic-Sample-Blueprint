# ssh-brute-force

## Scenario
An external host (45.134.26.7) ran an SSH brute force against the Linux server 10.10.2.22.  
After about 240 short failed sessions it opened one long session, the successful login.  

## Timeline (UTC+3)
```
11:05:00  Brute force starts from 45.134.26.7 to 10.10.2.22 port 22
11:1x:xx  ~240 short connections, each reset quickly after the banner exchange
later     One connection stays open and exchanges many packets, the compromise
```

## How to spot it
- One external IP opens hundreds of connections to port 22  
- Failed tries are tiny sessions that reset right after the SSH banner  
- The successful login is the one session that keeps exchanging data  
- All from a single foreign IP, unusual for an internal server  

## Questions
```
1. Attacker IP?                 45.134.26.7
2. Target and port?             10.10.2.22 port 22
3. About how many attempts?     ~240 failed, then 1 success
4. How do you tell success?     The long session that does not reset like the others
5. Service banner?              OpenSSH_8.9p1 Ubuntu
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 90480e2a7319973174e3cc1c1b589b3d156960189ce843f859788b328b280523.zip -phidden`. You get `ssh-brute-force.pcap`.
2. Open `ssh-brute-force.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `ip.addr==10.10.1.66`  (traffic to and from the attacker host). Then Statistics > Conversations to see the talkers.
4. Same filter from the shell with tshark: `tshark -r ssh-brute-force.pcap -Y "ip.addr==10.10.1.66"`, and list talkers with `tshark -r ssh-brute-force.pcap -q -z conv,ip`.
5. Confirm against the answer: One external IP opens hundreds of connections to port 22

## MITRE ATT&CK
T1110.001 Brute Force: Password Guessing  
T1021.004 Remote Services: SSH  

## Tools
Crafted SSH sessions (scapy) over generated background traffic  
Packets: 8406  

## Signature
Forensic Sample Blueprint  |  qayssarayra.com
