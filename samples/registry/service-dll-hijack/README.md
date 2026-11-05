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

## MITRE ATT&CK

- T1543.003 Windows Service
- T1574.002 DLL Side-Loading

## Tools

Registry Explorer, regedit, RECmd, reg query. Open the .reg in a text editor to read the keys directly.

## Signature

Forensic Sample Blueprint | qayssarayra.com
