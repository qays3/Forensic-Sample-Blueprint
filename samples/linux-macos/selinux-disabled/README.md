# selinux-disabled

## Scenario
An attacker sets SELinux to permissive to remove mandatory access control protections.

## Timeline
```
~15:57  setenforce 0 and config changed to permissive
```

## How to spot it
- setenforce 0 switching SELinux off
- Edit of /etc/selinux/config
- Protections dropped before further action
- Defense evasion

## Questions
```
1. Technique?          Disable SELinux
2. Command?            setenforce 0
3. Persistent change?  /etc/selinux/config
4. Category?           Defense evasion
5. Host?               dbsrv01
```

## MITRE ATT&CK
- T1562.001 Impair Defenses: Disable or Modify Tools

## Tools
Hand written Linux logs with a workday of normal activity as background

## Signature
Forensic Sample Blueprint | qayssarayra.com
