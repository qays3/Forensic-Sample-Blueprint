# systemd-service-persistence

## Scenario
An attacker installs a malicious systemd service that starts their binary at boot.

## Timeline
```
~14:15  New update-helper.service installed and started
```

## How to spot it
- A new .service unit in /etc/systemd/system
- systemctl enable --now of an unknown service
- A binary in /usr/local/bin run as root
- Survives reboot

## Questions
```
1. Technique?          systemd service persistence
2. Unit?               update-helper.service
3. Binary?             /usr/local/bin/update-helper
4. Enabled?            Yes (enable --now)
5. Host?               dbsrv01
```

## MITRE ATT&CK
- T1543.002 Create or Modify System Process: Systemd Service

## Tools
Hand written Linux logs with a workday of normal activity as background

## Signature
Forensic Sample Blueprint | qayssarayra.com
