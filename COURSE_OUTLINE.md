# COURSE OUTLINE — DC Lab Sim / NCP-AII Hands-on

> 12 modules · လေ့ကျင့်ခန်း 24+ · NCP-AII 5 domains အပြည့် mapping

---

## Module 00 — Orientation: DC Lab Sim နှင့် စတင်ခြင်း
**ရည်ရွယ်ချက်** — simulator ၏ UI, architecture ၆ မျိုး, mission/sandbox mode, terminal ကို ရင်းနှီးစွာ အသုံးပြုတတ်ရန်
- Cluster Health dashboard (Nodes/GPUs/InfiniBand/BCM HA)
- Architecture switcher (A100→VR200) — spec ပြောင်းလျှင် ဘာတွေ ပြောင်းလဲ
- Sandbox (fault inject, no score) vs Missions (scored) vs Quizzes
- လေ့လာနည်း loop: learn → inject → diagnose → remediate → verify

## Module 01 — nvidia-smi & GPU Telemetry
**ရည်ရွယ်ချက်** — `nvidia-smi` output တစ်ကွက်လုံး ဖတ်တတ်ရန်၊ GPU state မှန်/မမှန် ခွဲခြားရန် (D4/D5)
- Header (driver/CUDA version)၊ table columns တစ်ခုချင်း (Temp/Pwr/Mem/Util/ECC/Perf)
- `nvidia-smi -q`, `-d ECC/TEMPERATURE/POWER`, `nvidia-smi topo -m`, `nvidia-smi nvlink -s`
- utilization 0% vs 100% vs stuck၊ memory leak ပုံစံ၊ power cap ပြဿနာ
- dashboard telemetry နှင့် နှိုင်းယှဉ် ဖတ်ခြင်း

## Module 02 — DCGM & NVSM Monitoring
**ရည်ရွယ်ချက်** — cluster အဆင့် health tool များ အသုံးပြုတတ်ရန် (D4)
- `dcgmi discovery -l`, `dcgmi dmon -e 1001,1002,...`, `dcgmi diag -r 1/2/3`, `dcgmi health -s/-g`
- NVSM: `nvsm show health`, `nvsm show system`
- nvidia-smi (node) vs DCGM (cluster) vs NVSM (system rollup) — ဘယ်အခါ ဘာသုံး
- DCGM field IDs အခြေခံ (1001 GPU util, 1002 mem util...)

## Module 03 — XID Errors (Troubleshooting Core)
**ရည်ရွယ်ချက်** — XID/SXid ကုဒ်များ ဖတ်၍ root cause ရှာတတ်ရန် (D5 အဓိက)
- XID ဘာလဲ — `dmesg | grep -i xid`၊ အသုံးများ XID (13, 31, 43, 48, 63, 79, 94, 95, 119...)
- XID 43 (GPU hang) / 79 (fallen off bus) — ဖြေရှင်းပုံ ကွဲခြားချက်
- SXid (NVSwitch) နှင့် XID ကွာခြားချက်
- ဖြေရှင်း workflow: identify → isolate → reset/replace → validate

## Module 04 — ECC & Memory Errors
**ရည်ရွယ်ချက်** — ECC error အမျိုးအစားခွဲပြီး ကိုင်တွယ်တတ်ရန် (D5)
- correctable vs uncorrectable၊ `nvidia-smi -q -d ECC` ဖတ်နည်း
- `--reset-ecc-errors`၊ retired pages (page retirement/row remapping)
- uncorrectable → GPU replace (RMA) ဆုံးဖြတ်ချက်

## Module 05 — Thermal & Power Management
**ရည်ရွယ်ချက်** — အပူ/စွမ်းအင် fault များ ရှာဖွေ ဖြေရှင်းတတ်ရန် (D1.5, D5)
- `nvidia-smi -q -d TEMPERATURE/POWER`, `ipmitool sensor list`
- thermal throttling (Perf P0→P2/P3)၊ power limit exceeded
- datacenter cooling အခြေခံ — လေအေး vs liquid cooling, hot/cold aisle
- power cap config: `nvidia-smi -pl`

## Module 06 — NVLink & InfiniBand Fabric
**ရည်ရွယ်ချက်** — intra-node fabric (NVLink) နှင့် inter-node fabric (IB) fault ရှာတတ်ရန် (D4/D5)
- `nvidia-smi nvlink -s/-e`, topology view, NVLink generation 6 မျိုး (12/18 links)
- `ibstat`, link state (Active/Down), rate (HDR 200G → XDR 800G)
- fabric manager (`systemctl restart nvidia-fabricmanager`)၊ NVSwitch
- link degradation → diagnose → reseat/replace

## Module 07 — BMC, OOB & Physical Repair
**ရည်ရွယ်ချက်** — out-of-band management နှင့် physical intervention လုပ်တတ်ရန် (D1)
- BMC/OOB/TPM အခြေခံ၊ `ipmitool sensor list`, `ipmitool chassis power cycle`
- `dmidecode` — hardware inventory ဖတ်နည်း
- reseat GPU / NVLink bridge / mark for RMA — ဘယ်အခါ ဘာလုပ်
- bring-up sequence (deployment → validation အစဉ်)

## Module 08 — Slurm & Workload Simulation
**ရည်ရွယ်ချက်** — job queue အခြေအနေ ဖတ်ပြီး workload အလိုက် performance ခွဲခြားတတ်ရန် (D4)
- `sinfo`, node state (idle/alloc/down/drain)
- workload ၄ မျိုး (Idle 5% / Inference 60% / Training 95% / Stress 100%) — expected output ကွာခြားချက်
- utilization ဖတ်ခြင်း — healthy vs bottleneck vs fault

## Module 09 — Complex Multi-Symptom Scenarios
**ရည်ရွယ်ချက်** — symptom ပေါင်းစုံပါ failure ကို systematic ဖြေရှင်းတတ်ရန် (D5)
- GPU Hang (XID 43) / Bus Reset (XID 79) / Thermal Alert (90-100°C) / Severe ECC
- systematic workflow: gather → hypothesize → test → fix → verify
- documentation/report (nvidia-bug-report.sh)

## Module 10 — 32 Missions Playbook
**ရည်ရွယ်ချက်** — missions အားလုံး ကျွမ်းကျင်စွာ ဖြတ်နိုင်ရန်
- mission အမျိုးအစားများ (bring-up, validate, troubleshoot, optimize)
- 229 commands playbook — အသုံးများ command family များ
- story hook ဖတ်နည်း — "အရိပ်အမြွက်" မှ ဆုံးဖြတ်ချက်

## Module 11 — Exam Readiness (NCP-AII)
**ရည်ရွယ်ချက်** — စာမေးပွဲ အဆင်သင့် ဖြစ်ကြောင်း အတည်ပြုရန်
- 5 domains အလိုက် weight ဖြင့် ပြန်လည် သုံးသပ်ခြင်း
- Top exam traps (platform-specific procedure, enabled vs usable, sequence, tool output)
- 120 မိနစ် / 70-75 မေးခွန်း time management
- exam-day checklist + နောက်ဆုံး practice loop

---

## Module 12 — AI Factory Topology, Cabling & Storage (Knowledge)
**ရည်ရွယ်ချက်** — simulator ထဲ မပါသော D1 theory ဖြည့်ရန် (D1.2, D1.8, D1.11)
- Compute/Storage/Management fabric; fat-tree, rail-optimized topology
- DAC/AOC/optical transceiver ရွေးချယ်မှု; signal quality validation
- Parallel storage architecture; storage test နှင့် data pipeline

## Module 13 — BlueField DPU & MIG (Knowledge)
**ရည်ရွယ်ချက်** — D2 (5%) အပြည့် (D2.1, D2.2)
- DPU concept, offload, DOCA platform; BlueField modes of operation
- MIG hardware partition; configuration command sequence; "enabled ≠ usable" trap

## Module 14 — Control Plane Stack (Knowledge)
**ရည်ရွယ်ချက်** — D3 (19%) software stack (D3.1-3.7)
- BCM (+HA), OS/driver install (GPU/DOCA), Slurm + Enroot + Pyxis
- NVIDIA Container Toolkit (Docker + GPU verify), NGC CLI

## Module 15 — Cluster Test & Verification Deep (Knowledge)
**ရည်ရွယ်ချက်** — D4 (33%) test တစ်ခုချင်း၏ ရည်ရွယ်ချက် (D4.1-4.14)
- HPL, NCCL (single-node + E/W), ClusterKit, NeMo burn-in, storage test
- Cable signal/FW validation; burn-in philosophy

---

**စုစုပေါင်း လေ့လာချိန်** — ၂-၃ ပတ် (တစ်ရက် ၁-၂ နာရီ) သို့မဟုတ် ၁ ပတ် အပြည့် (bootcamp)
