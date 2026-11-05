# Port Monitor Dll

## Scenario

A print port monitor is registered with a DLL driver, loaded by the spooler service as SYSTEM at boot.

Host: WS-FIN-07  
Evidence: `registry.reg` (exported registry keys)  
Approx hive last write: 2019-06-02 11:20

## Timeline

A new Monitors subkey with a Driver DLL that spooler loads.

## How to spot it

- A new Print\Monitors subkey
- A Driver value naming a DLL in System32
- Loaded by spoolsv.exe as SYSTEM
- Survives reboot via the spooler

## Questions

```
1. Key?               Print\Monitors
2. Driver?             evilmon.dll
3. Loaded by?           Spooler (SYSTEM)
4. At?                 Boot
5. Technique?          Port monitor
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x a7e323fc1971a8c692b22f76fbee621715c43bc4f344c037f83524a91bd0b586.zip -phidden` (or your archive tool). You get `registry.reg`.
2. Read it as text: `cat registry.reg`, or import into a throwaway hive and open in Registry Explorer / RECmd.
3. Jump to the key that matters: `grep -niA2 -E 'Run|AppInit|Userinit|Debugger|ServiceDll|Security Packages|AlwaysInstallElevated|DefaultPassword|USBSTOR' registry.reg` (narrow to this one).
4. Confirm against the answer: A new Print\Monitors subkey

## MITRE ATT&CK

- T1547.010 Port Monitors

## Tools

Registry Explorer, regedit, RECmd, reg query. Open the .reg in a text editor to read the keys directly.

## Signature

Forensic Sample Blueprint | qayssarayra.com
