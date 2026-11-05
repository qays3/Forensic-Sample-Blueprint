# Disable Powershell Logging Reg

## Scenario

PowerShell script block and module logging are turned off via policy keys, blinding a key detection source before running tooling.

Host: WS-FIN-07  
Evidence: `registry.reg` (exported registry keys)  
Approx hive last write: 2021-12-04 13:17

## Timeline

ScriptBlockLogging and ModuleLogging policy values set to 0.

## How to spot it

- EnableScriptBlockLogging set to 0
- EnableModuleLogging set to 0
- Done before running PowerShell tooling
- Removes 4104 visibility

## Questions

```
1. Keys?              PowerShell ScriptBlock/Module logging
2. Set to?             0 (disabled)
3. Effect?             No 4104 logging
4. When?               Before tooling
5. Technique?          Impair defenses
```

## MITRE ATT&CK

- T1562.002 Disable Windows Event Logging

## Tools

Registry Explorer, regedit, RECmd, reg query. Open the .reg in a text editor to read the keys directly.

## Signature

Forensic Sample Blueprint | qayssarayra.com
