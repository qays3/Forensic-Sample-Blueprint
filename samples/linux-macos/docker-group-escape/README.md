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

## MITRE ATT&CK
- T1611 Escape to Host

## Tools
Hand written Linux logs with a workday of normal activity as background

## Signature
Forensic Sample Blueprint | qayssarayra.com
