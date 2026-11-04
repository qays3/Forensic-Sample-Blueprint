# ld-preload-persistence

## Scenario
An attacker adds a malicious shared object to /etc/ld.so.preload so it loads into every process.

## Timeline
```
~14:25  /etc/ld.so.preload points to /lib/.hook.so
```

## How to spot it
- A new entry in /etc/ld.so.preload
- A hidden .so loaded system wide
- Injected into every dynamically linked process
- Userland rootkit style persistence

## Questions
```
1. File?               /etc/ld.so.preload
2. Library?            /lib/.hook.so
3. Effect?             Loaded into all processes
4. Technique?          LD_PRELOAD rootkit
5. Host?               dbsrv01
```

## MITRE ATT&CK
- T1574.006 Hijack Execution Flow: Dynamic Linker Hijacking

## Tools
Hand written Linux logs with a workday of normal activity as background

## Signature
Forensic Sample Blueprint | qayssarayra.com
