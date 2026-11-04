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

## MITRE ATT&CK
- T1496 Resource Hijacking

## Tools
Hand written Linux logs with a workday of normal activity as background

## Signature
Forensic Sample Blueprint | qayssarayra.com
