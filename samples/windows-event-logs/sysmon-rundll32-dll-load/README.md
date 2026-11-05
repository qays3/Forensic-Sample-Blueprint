# Sysmon Rundll32 Dll Load

## Scenario

rundll32 loads a DLL from a user writable path, a common execution and defense evasion trick, caught by Sysmon id 1.

Host: DESKTOP-V96574F  
Evidence: `sysmon-operational.evtx` (real Windows Sysmon EVTX, one chunk)  
Channel: Microsoft-Windows-Sysmon/Operational

## Timeline

A Sysmon id 1 for rundll32.exe loading a DLL from C:\Users\Public.

## How to spot it

- Sysmon 1 rundll32.exe
- CommandLine loads a DLL from a user path
- Export name on the command line
- rundll32 abused as a proxy

## Questions

```
1. Log?               Sysmon 1
2. Binary?             rundll32.exe
3. CommandLine?         C:\Users\Public\a.dll,DllRegisterServer
4. Tell?               DLL in a user path
5. Technique?          Signed binary proxy
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 58f524c6933473704d6ad09c4ae45a7b0ed2432b029379ff12b6c3c49b11eeaf.zip -phidden`. You get `sysmon-operational.evtx`.
2. Open it in Event Viewer: Action > Open Saved Log, or parse it.
3. Filter and read: `Get-WinEvent -Path sysmon-operational.evtx | Where-Object Id -in 1,3 | Format-List TimeCreated,Id,Message`, or `EvtxECmd.exe -f sysmon-operational.evtx --csv . --csvf out.csv`, or `wevtutil qe sysmon-operational.evtx /lf:true /f:text`.
4. Find the malicious record among the normal Sysmon events.
5. Confirm against the answer: Sysmon 1 rundll32.exe

## MITRE ATT&CK

- T1218.011 Rundll32

## Tools

Event Viewer, Get-WinEvent, EvtxECmd, Chainsaw, Sysmon View, Timeline Explorer.

## Signature

Forensic Sample Blueprint | qayssarayra.com
