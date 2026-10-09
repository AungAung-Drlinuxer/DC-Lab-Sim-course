# Module 15 — Solution

## E1
- HPL: sustained FP64 compute + thermal
- NCCL single-node: NVLink/NVSwitch bandwidth
- NCCL E/W: node ကြား fabric bandwidth
- ClusterKit: node-to-node multifaceted assessment
- NeMo burn-in: end-to-end training path (storage/data)
- Storage: read/write throughput

## E2
1. cable/FW validate → 2. single-node stress → 3. HPL → 4. NCCL single-node → 5. NCCL E/W + ClusterKit → 6. burn-in (HPL/NCCL/NeMo) + storage test

## E3
1. link state/error counter (`ibstat`, `nvidia-smi nvlink -e`)
2. NCCL config (protocol, topology env) နှင့် power/thermal throttle
3. topology (rail mapping) နှင့် switch config (SHARP/QoS)
