# Service Imagepath Modified

## Scenario

A known service ImagePath is repointed at an attacker binary so a trusted service name runs malicious code.

Host: WS-FIN-07  
Evidence: `registry.reg` (exported registry keys)  
Approx hive last write: 2024-03-22 12:40

## Timeline

A service (Spooler) with an ImagePath changed to a ProgramData binary.

## How to spot it

- A well known service with an ImagePath outside System32
- Auto start kept so it runs at boot
- Trusted service name, untrusted path
- Change on an existing service, not a new one

## Questions

```
1. Artifact?          Services key ImagePath
2. Service?            A known one (Spooler)
3. New path?           ProgramData\p\svc.exe
4. Start?              Auto (2)
5. Technique?          Service modification
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x b407130d05c2d8b2637d5fea691482b17b00829ec9c8e63bcbaf3b89731c6620.zip -phidden` (or your archive tool). You get `registry.reg`.
2. Read it as text: `cat registry.reg`, or import into a throwaway hive and open in Registry Explorer / RECmd.
3. Jump to the key that matters: `grep -niA2 -E 'Run|AppInit|Userinit|Debugger|ServiceDll|Security Packages|AlwaysInstallElevated|DefaultPassword|USBSTOR' registry.reg` (narrow to this one).
4. Confirm against the answer: A well known service with an ImagePath outside System32

## MITRE ATT&CK

- T1543.003 Windows Service
- T1574 Hijack Execution Flow

## Tools

Registry Explorer, regedit, RECmd, reg query. Open the .reg in a text editor to read the keys directly.

## Signature

Forensic Sample Blueprint | qayssarayra.com
