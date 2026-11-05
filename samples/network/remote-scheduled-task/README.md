# Remote Scheduled Task

## Scenario

A scheduled task is created remotely from 10.10.1.66 on 10.10.2.24: the taskschd DCERPC interface over SMB registers and runs a task.

Evidence: `remote-scheduled-task.pcap`  
Capture date: 2025-05-18

## Timeline

~10:08  taskschd DCERPC over IPC$, SchRpcRegisterTask then SchRpcRun.

## How to spot it

- taskschd (ITaskSchedulerService) over DCERPC
- SchRpcRegisterTask in the traffic
- Task runs a command
- Created over SMB named pipe

## Questions

```
1. Interface?          taskschd
2. Call?               SchRpcRegisterTask
3. Transport?          DCERPC over SMB
4. Action?             run a task
5. ATT&CK?             T1053.005
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x <sha256>.zip -phidden`. You get `remote-scheduled-task.pcap`.
2. Open `remote-scheduled-task.pcap` in Wireshark.
3. In Wireshark, paste this into the display filter bar and press Enter: `dcerpc || tcp.port==445`  (the taskschd calls). Then Statistics > Conversations for the talkers.
4. Same filter from the shell: `tshark -r remote-scheduled-task.pcap -Y "dcerpc || tcp.port==445"`, and `tshark -r remote-scheduled-task.pcap -q -z conv,ip`.
5. Confirm against the answer: taskschd (ITaskSchedulerService) over DCERPC

## MITRE ATT&CK

- T1053.005 Scheduled Task
- T1021.002 SMB/Admin Shares

## Tools

Wireshark, tshark, zeek, tcpdump.

## Signature

Forensic Sample Blueprint | qayssarayra.com
