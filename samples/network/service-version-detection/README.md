# service-version-detection

## Scenario
10.10.1.66 ran service and version detection against open ports on 10.10.2.15, grabbing  
banners to identify the software.  

## How to spot it
- Short connections to known service ports that pull a banner then probe  
- Banners reveal OpenSSH 8.9, Apache 2.4.52, vsFTPd 3.0.5, Postfix, MySQL 8.0.34  
- Follows a port scan in a real attack  

## Questions
```
1. Scanner IP?        10.10.1.66
2. What is grabbed?   Service banners and versions
3. SSH version?       OpenSSH_8.9p1
4. Web server?        Apache 2.4.52
```

## MITRE ATT&CK
T1046 Network Service Discovery  

## Signature
Forensic Sample Blueprint  |  qayssarayra.com
