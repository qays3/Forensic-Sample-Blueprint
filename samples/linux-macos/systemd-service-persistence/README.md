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

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 73c56d21fc265837dd61715add27d6cfc58c54c313ed980c91460f704ccf8464.zip -phidden` (or your archive tool). You get `auth.log`.
2. Read the log: `less auth.log`; for auditd use `ausearch -if auth.log`, for a journal export `journalctl --file=auth.log`.
3. Grep the indicator: `grep -Ei 'failed|accepted|sudo|useradd|/tmp/|preload|cron|wget|curl' auth.log` (narrow to this attack).
4. Confirm against the answer: A new .service unit in /etc/systemd/system

## MITRE ATT&CK
- T1543.002 Create or Modify System Process: Systemd Service

## Tools
Hand written Linux logs with a workday of normal activity as background

## Signature
Forensic Sample Blueprint | qayssarayra.com
