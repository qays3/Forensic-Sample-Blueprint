# cryptominer

## Scenario
An attacker drops and runs XMRig, which mines Monero to a pool and pins the CPU.

## Timeline
```
~14:33  xmrig started, connecting to a mining pool, CPU maxed
```

## How to spot it
- A process named xmrig or similar miner
- Connections to a mining pool (stratum)
- Sustained high CPU
- Dropped from an attacker host

## Questions
```
1. Malware?            XMRig (cryptominer)
2. Pool?               pool.minexmr.com:4444
3. Symptom?            High CPU
4. Dropped from?       45.137.22.90
5. Host?               dbsrv01
```

## How to solve it

1. Extract the evidence, password `hidden`: `7z x 993a5ce3f7aa7d195c9671713f4ad9e9b7c2cbaf2b89b6fcc2603f2c86fb041b.zip -phidden` (or your archive tool). You get `auth.log`.
2. Read the log: `less auth.log`; for auditd use `ausearch -if auth.log`, for a journal export `journalctl --file=auth.log`.
3. Grep the indicator: `grep -Ei 'failed|accepted|sudo|useradd|/tmp/|preload|cron|wget|curl' auth.log` (narrow to this attack).
4. Confirm against the answer: A process named xmrig or similar miner

## MITRE ATT&CK
- T1496 Resource Hijacking

## Tools
Hand written Linux logs with a workday of normal activity as background

## Signature
Forensic Sample Blueprint | qayssarayra.com
