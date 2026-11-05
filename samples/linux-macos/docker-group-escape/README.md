# docker-group-escape

## Scenario
A user in the docker group mounts the host filesystem into a container to read root files, escaping to the host.

## Timeline
```
~14:05  docker run with -v /:/host mounts the whole host
```

## How to spot it
- A user in the docker group running docker
- A container mounting / from the host
- chroot into the host filesystem
- Reading host /etc/shadow from the container

## Questions
```
1. Group abused?       docker
2. Technique?          Mount host / into container
3. Command?            docker run -v /:/host
4. Goal?               Host root access
5. Host?               appsrv02
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 0484466af40310174f8734a60fdf713f1d4d5be054e869cd3aa44133e7a2cfc9.zip -phidden` (or your archive tool). You get `auth.log`.
2. Read the log: `less auth.log`; for auditd use `ausearch -if auth.log`, for a journal export `journalctl --file=auth.log`.
3. Grep the indicator: `grep -Ei 'failed|accepted|sudo|useradd|/tmp/|preload|cron|wget|curl' auth.log` (narrow to this attack).
4. Confirm against the answer: A user in the docker group running docker

## MITRE ATT&CK
- T1611 Escape to Host

## Tools
Hand written Linux logs with a workday of normal activity as background

## Signature
Forensic Sample Blueprint | qayssarayra.com
