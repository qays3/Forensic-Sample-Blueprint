# arp-sweep

## Scenario
10.10.1.66 ran an ARP sweep across the local subnet 10.10.1.0/24 to map live hosts.  

## How to spot it
- A burst of ARP who-has requests for every address on the subnet from one MAC  
- Only works on the local segment, so a sign of a host already inside  
- Live hosts reply with ARP is-at  

## Questions
```
1. Scanner IP?      10.10.1.66
2. Method?          ARP who-has sweep
3. Why local only?  ARP does not cross routers
4. Live hosts?      10
```

## MITRE ATT&CK
T1018 Remote System Discovery  

## Signature
Forensic Sample Blueprint  |  qayssarayra.com
