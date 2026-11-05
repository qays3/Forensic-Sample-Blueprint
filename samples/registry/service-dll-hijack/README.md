# Service Dll Hijack

## Scenario

A svchost hosted service has its Parameters ServiceDll set to an attacker DLL, so svchost loads malicious code under a service.

Host: WS-FIN-07  
Evidence: `registry.reg` (exported registry keys)  
Approx hive last write: 2022-01-12 13:59

## Timeline

A service running under svchost with a ServiceDll pointing at ProgramData.

## How to spot it

- A Parameters\ServiceDll outside System32
- Hosted by svchost -k netsvcs
- Malicious DLL loaded by a trusted host process
- Service name close to a real one

## Questions

```
1. Artifact?          Parameters ServiceDll
2. Host?               svchost -k netsvcs
3. DLL?                ProgramData\wh\helper.dll
4. Effect?             Code in svchost
5. Technique?          Service DLL hijack
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x cb9a5031d48521bd0ca421ca73f0ce8854c236ef6f4090991459617458e75ea4.zip -phidden` (or your archive tool). You get `registry.reg`.
2. Read it as text: `cat registry.reg`, or import into a throwaway hive and open in Registry Explorer / RECmd.
3. Jump to the key that matters: `grep -niA2 -E 'Run|AppInit|Userinit|Debugger|ServiceDll|Security Packages|AlwaysInstallElevated|DefaultPassword|USBSTOR' registry.reg` (narrow to this one).
4. Confirm against the answer: A Parameters\ServiceDll outside System32

## MITRE ATT&CK

- T1543.003 Windows Service
- T1574.002 DLL Side-Loading

## Tools

Registry Explorer, regedit, RECmd, reg query. Open the .reg in a text editor to read the keys directly.

## Signature

Forensic Sample Blueprint | qayssarayra.com
