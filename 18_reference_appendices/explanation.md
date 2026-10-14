# Module 18 — Reference Appendices (XID / Glossary / Command Family)

> ဤ module သည် သင်ခန်းစာ chapter မဟုတ်ပါ — **reference dictionary** ဖြစ်ပါသည်။
> Module 00–15 ကို လေ့ကျင့်ရင်း အဖြေရှာရာတွင် ပြန်လှန်ကြည့်ရန် ဖြစ်ပါသည်။

## 1. ဤ appendices ကို ဘယ်လို သုံးမလဲ

- **Appendix A (XID Reference)** — `dmesg | grep -i xid` ထဲက ကုဒ် တွေ့ပါက ဤနေရာတွင် ရှာပါ။
  Severity ကို ကြည့်ပြီး "ချက်ချင်း ဆောင်ရွက် / စောင့်ကြည့် / မှတ်တမ်းသာ" ခွဲပါ။
- **Appendix B (Glossary)** — စာမေးပွဲနှင့် mission များတွင် တွေ့ရသော ဝေါဟာရ အတိုကောက်
  (MIG, GRES, SHARP...) ၏ အဓိပ္ပာယ် ပြန်ရှာရာ ဖြစ်ပါသည်။
- **Appendix C (Command Family Reference)** — "ဒီလုပ်ငန်းအတွက် ဘယ် tool family သုံးရမလဲ"
  ဆိုသည့် မေးခွန်းအမျိုးအစား (tool selection) အတွက် ဖြစ်ပါသည်။

**လေ့ကျင့်နည်း** — ပထမဆုံး အကြိမ်တွင် open-book အနေဖြင့် exercise ဖြေရင်း table ထဲ ရှာပါ။
တတိယအကြိမ် ပြန်လေ့ကျင့်ချိန်တွင် table မကြည့်ဘဲ ဖြေကြည့်ပြီးမှ ပြန်စစ်ပါ —
ရှာတတ်ခြင်းနှင့် မှတ်မိခြင်း ၂ မျိုးလုံးက စာမေးပွဲတွင် လိုအပ်ပါသည်။

---

## Appendix A — XID Error Code Reference (ကုဒ် ၂၈ ခု)

> ဤဇယားသည် **dclabsim.com Docs** တွင် ဖော်ပြထားသော XID reference ဖြစ်ပြီး
> NVIDIA ၏ လက်တွေ့အပြုအမူကို **ပညာရေးဆိုင်ရာ approximation** အဖြစ် ဖော်ပြခြင်း ဖြစ်ပါသည်။
> လက်တွေ့ datacenter တွင် ကုဒ်တစ်ခုချင်း၏ အဓိပ္ပာယ်နှင့် ဖြေရှင်းနည်းကို
> NVIDIA ၏ XID documentation (https://docs.nvidia.com/deploy/xid-errors/) အတိုင်း
> လိုက်နာပါ — driver version အလိုက် အသေးစိတ် ကွဲနိုင်ပါသည် (Module 03 နှင့်လည်း ဆက်စပ်ပါသည်)။

### Severity အကျဉ်းချုပ်

| Severity | အဓိပ္ပာယ် | ကုဒ်အရေအတွက် |
|---|---|---|
| **Critical** | ချက်ချင်း ဆောင်ရွက်ရန် — job ရပ်၊ node drain၊ hardware စစ်/RMA အထိ | 13 |
| **Warning** | စောင့်ကြည့်ရန် — ဖြစ်ပွားမှုနှုန်း မှတ်တမ်းတင်၊ app/driver ဘက်မှ စစ် | 13 |
| **Informational** | မှတ်တမ်းသာ — system က ကိုင်တွယ်ပြီးသား ဖြစ်နိုင် | 2 |

⭐ = dclabsim.com တွင် **[Exam]** tag ပါသော ကုဒ် (စာမေးပွဲတွင် မေးနိုင်ခြင်း ပိုများ)။

### A.1 Critical — ချက်ချင်း ဆောင်ရွက်ရန်

| Code | အမည် | အဓိပ္ပာယ် | Severity | Exam |
|---|---|---|---|---|
| **27** | GPU Memory Interface Error | GPU core နှင့် VRAM ကြား ဆက်သွယ်မှု ပျက်သွားခြင်း | Critical | ⭐ |
| **38** | Driver Firmware Mismatch | driver နှင့် firmware version မကိုက်ညီခြင်း — version ပြန်ညှိ/update | Critical | ⭐ |
| **48** | Double-Bit ECC Error | uncorrectable ECC (DBE) — data ပြန်လည် မရနိုင်တော့ | Critical | ⭐ |
| **54** | Auxiliary Power Connector Not Connected | GPU auxiliary power connector မချိတ်ရသေး/ပြတ်နေခြင်း | Critical | ⭐ |
| **62** | PMU Halt Error | GPU state ပြန်လည် မရနိုင်တော့အောင် ပျက်သွားခြင်း | Critical | — |
| **64** | Row Remapping Failure | INFOROM remap table ပြည့်/ပျက် — row remap မှတ်တမ်းတင်၍ မရခြင်း | Critical | ⭐ |
| **74** | NVLink Error | NVLink ချိတ်ဆက်မှု အမှား | Critical | ⭐ |
| **76** | ROBUST_CHANNEL_CE7_ERROR | NVLink link training မအောင်မြင်ခြင်း | Critical | ⭐ |
| **77** | NVLink Timeout | NVLink ဆက်သွယ်မှု timeout ဖြစ်ခြင်း | Critical | ⭐ |
| **78** | NVLink ECC Error | NVLink လမ်းကြောင်းပေါ် uncorrectable ECC | Critical | ⭐ |
| **79** | GPU Fallen Off Bus | GPU ကို PCIe bus က မမြင်တော့ — GPU မတုံ့ပြန်တော့ခြင်း | Critical | ⭐ |
| **95** | Uncontained ECC Error | uncontained ECC — data ပျက်စီးမှု ဖြစ်နိုင်ခြင်း | Critical | ⭐ |
| **119** | GSP Error | GPU System Processor error | Critical | — |

### A.2 Warning — စောင့်ကြည့်ရန်

| Code | အမည် | အဓိပ္ပာယ် | Severity | Exam |
|---|---|---|---|---|
| **13** | Graphics Engine Exception | shader execution exception — application error အများစု | Warning | ⭐ |
| **23** | GPU Shared Memory Exception | shared memory access violation | Warning | — |
| **24** | GPU Exception During Kernel Launch | kernel launch အတွင်း GPU exception | Warning | — |
| **31** | GPU Memory Page Fault | GPU memory page fault | Warning | — |
| **32** | Invalid or Corrupted Push Buffer | push buffer မမှန်/ပျက်နေခြင်း | Warning | — |
| **43** | GPU Stopped Responding | app hang — GPU က အလုပ်လုပ်နိုင်သေး၊ context သာ ပျက်သွားခြင်း | Warning | ⭐ |
| **56** | Display Engine Error | display engine အမှား | Warning | — |
| **57** | Error in Copy Engine | copy engine အမှား | Warning | — |
| **68** | Video Processor Exception | video processor exception | Warning | — |
| **69** | Graphics Engine Class Error | graphics engine class အမှား | Warning | — |
| **72** | ROBUST_CHANNEL_CE5_ERROR | NVLink flow control credits ကုန်သွားခြင်း | Warning | — |
| **92** | High Single-Bit ECC Rate | ပြင်လို့ရသော (correctable) ECC နှုန်း မြင့်တက်နေခြင်း | Warning | ⭐ |
| **94** | Contained ECC Error | contained ECC — data ကို မထိခိုက်ခြင်း | Warning | — |

### A.3 Informational — မှတ်တမ်းသာ

| Code | အမည် | အဓိပ္ပာယ် | Severity | Exam |
|---|---|---|---|---|
| **45** | Preemptive GPU Cleanup | GPU ကို ကြိုတင် ရှင်းလင်းခြင်း | Informational | — |
| **63** | Row Remapping Success | ပျက်သော row ကို spare row သို့ အောင်မြင်စွာ remap ပြုလုပ်ပြီးခြင်း | Informational | — |

### A.4 Triage အစဉ် — ကုဒ် ၄ ခုအတွက် အမြန်စည်းမျဉ်း ⭐

Module 03 က သင်ထားပြီးသား workflow (Identify → Isolate → Classify → Remediate → Validate → Document)
အတိုင်း ဆက်လက် ဆောင်ရွက်ပါ။ အောက်ပါ ၄ ခုက အမှားမခံနိုင်ဆုံး ကုဒ်များ ဖြစ်ပါသည်။

```
XID 79 (Critical) — GPU fallen off the bus
  GPU ကို bus ပေါ် မမြင်တော့ → hardware/link ပြဿနာ
  → evidence ရယူ (dmesg, nvidia-bug-report.sh) → reseat GPU/PCIe
    → server power cycle → မကောင်းပါက RMA
  ⚠️ software reset နှင့်သာ ဖြေရှင်းခြင်းက ပြဿနာကို ဖုံးကွယ်စေသည်

XID 43 (Warning) — GPU stopped responding (hang)
  GPU က ရှိနေသေး၊ bus ပေါ် ရှိနေသေး — context သာ ပျက်ခြင်း
  → GPU ပေါ်က process များ ရှင်းပြီး nvidia-smi --gpu-reset (software reset)
  ⚠️ ချက်ချင်း hardware ထိခြင်းက အချိန် ဆုံးရှုံးစေသည်

XID 48 (Critical) — Double-bit ECC (uncorrectable)
  data ပြန်လည် မရနိုင် — job result ကို ယုံ၍ မရတော့
  → job ရပ်/restart၊ ECC state မှတ်တမ်းတင်၊ GPU replace (RMA) စဉ်းစား

XID 95 (Critical) — Uncontained ECC
  data ပျက်စီးမှု ဖြစ်နိုင် → job ရပ်၊ GPU reset/replace အဆင့် စဉ်းစား
  ⚠️ XID 94 (contained — data မထိခိုက်) နှင့် မရောမိပါစေနှင့်
```

**အထက်ပါ အားလုံးမတိုင်မီ** — `dmesg | grep -i xid` output နှင့် `nvidia-bug-report.sh` ကို
အရင် ရယူပါ။ reset ပြုလုပ်ပြီးပါက evidence များ ပျောက်သွားနိုင်ပါသည် (Module 03 §7)။

---

## Appendix B — Glossary (ဝေါဟာရ ၈၀)

dclabsim.com Docs > Glossary မှ ဝေါဟာရ အားလုံးကို အမျိုးအစား ၅ မျိုးခွဲ၍
အဓိပ္ပာယ် ဖော်ပြထားပါသည်။ အဓိပ္ပာယ်များကို NVIDIA documentation နှင့် စက်မှုလုပ်ငန်း
အဓိပ္ပာယ်ဖွင့်ဆိုချက်များအတိုင်း ရေးသားထားပါသည်။

### B.1 GPU & Compute (၂၇ ခု)

| ဝေါဟာရ | အဓိပ္ပာယ် | မှတ်စု |
|---|---|---|
| CRC | Cyclic Redundancy Check — data ပျက်ခြင်း စစ်ဆေးသော checksum | link အရည်အသွေး တိုင်းတာရာတွင် သုံး |
| CUDA | NVIDIA ၏ parallel computing platform + programming model/API | GPU ကို general computation အတွက် သုံးခြင်း |
| cuDNN | CUDA Deep Neural Network library — GPU-accelerated deep learning primitives | convolution, pooling စသည့် building blocks |
| DBE | Double-Bit Error — uncorrectable ECC error | XID 48 နှင့် ဆက်စပ် |
| DCGM | Data Center GPU Manager — cluster အဆင့် GPU telemetry/diagnostics/health | `dcgmi` CLI (Module 02) |
| DGX | NVIDIA ၏ integrated AI server စီးရီး (ဥပမာ DGX A100 — 8 GPU) | simulator ရဲ့ node များလည်း ဖြစ် |
| DRAM | Dynamic RAM — volatile memory | GPU တွင် HBM အဖြစ် တွေ့ရ |
| ECC | Error Correction Code — memory error စစ်/ပြင်ခြင်း | single-bit = correctable, double-bit = uncorrectable |
| FP16 / FP32 / FP64 | 16/32/64-bit floating-point format | bit နည်း = မြန်/သိပ်သည်း၊ precision ကျ |
| GPU | Graphics Processing Unit — massively parallel processor | AI training/inference accelerator |
| GSP | GPU System Processor — driver လုပ်ငန်းများ ခွဲဝေ ထမ်းဆောင်ပေးသော on-GPU processor | XID 119 |
| HBM | High Bandwidth Memory — GPU die ဘေးတွင် stack လုပ်ထားသော DRAM | bandwidth အလွန်မြင့် |
| MIG | Multi-Instance GPU — GPU တစ်လုံးကို isolated instance များ ခွဲခြင်း | "enabled ≠ usable" trap (Module 13) |
| MPS | Multi-Process Service — CUDA process များ GPU ကို တစ်ပြိုင်နက် မျှဝေသုံးခြင်း | context switch ကုန်ကျစရိတ် လျော့ |
| NVLink | GPU↔GPU (GPU↔CPU) high-speed interconnect | intra-node fabric (Module 06) |
| NVML | NVIDIA Management Interface — monitoring/management C library | `nvidia-smi` ရဲ့ အောက်ခံ API |
| NVSM | NVIDIA System Manager — DGX system-level health tool | `nvsm show health` (Module 02) |
| NVSwitch | NVLink ဖြင့် GPU အားလုံး all-to-all ချိတ်ပေးသော switch chip | DGX/HGX အတွင်း |
| SM | Streaming Multiprocessor — GPU ၏ အခြေခံ compute unit | ⚠️ Networking ထဲက SM (Subnet Manager) နှင့် မတူ |
| SRAM | Static RAM — on-chip အမြန် memory (registers, caches, shared memory) | |
| SXM | NVIDIA GPU module form factor (SXM4/5...) — NVLink ချိတ်ဆက်မှု ပါ | PCIe card မဟုတ် |
| TDP | Thermal Design Power — cooling စနစ် ကိုင်တွယ်ရမည့် အပူ/စွမ်းအင် ဒီဇိုင်းပမာဏ | power cap နှင့် ဆက်စပ် |
| TFLOPS | Tera (10¹²) FLOPS — floating-point ops/sec | HPL ရလဒ် တိုင်းတာရာတွင် သုံး |
| VBIOS | GPU firmware — board init, clocks, power limit ထိန်းချုပ် | firmware validation တွင် စစ် |
| XID | driver က kernel ကို ပေးသော error event code | Appendix A / Module 03 |

### B.2 Networking & InfiniBand (၁၅ ခု)

| ဝေါဟာရ | အဓိပ္ပာယ် | မှတ်စု |
|---|---|---|
| GDS | GPUDirect Storage — storage data ကို CPU RAM မဖြတ်ဘဲ GPU memory သို့ တိုက်ရိုက် ပို့ခြင်း | data pipeline အတွက် |
| GID | Global Identifier — RDMA endpoint ၏ IPv6 အခြေခံ address | RoCE/IB |
| GUID | Globally Unique Identifier — HCA/port တစ်ခုချင်း၏ hardware ID | MAC address နှင့် ဆင်တူ |
| HCA | Host Channel Adapter — host ထဲက InfiniBand NIC | ဥပမာ ConnectX |
| HDR | High Data Rate — IB speed generation (port တစ်ခုလျှင် 200 Gb/s) | |
| IB | InfiniBand — RDMA အခြေခံ high-speed, low-latency cluster interconnect | |
| LID | Local Identifier — Subnet Manager က ခွဲဝေပေးသော 16-bit local address | subnet အတွင်းသာ |
| NDR | Next Data Rate — IB generation (port တစ်ခုလျှင် 400 Gb/s) | နောက်တစ်ဆင့် XDR 800G |
| NFS | Network File System — network ပေါ် file sharing protocol | |
| OFED | OpenFabrics Enterprise Distribution — Linux RDMA/IB software stack | driver, library, tool များ |
| QoS | Quality of Service — fabric ပေါ် traffic prioritization / virtual lanes | |
| RDMA | Remote Direct Memory Access — kernel bypass ဖြင့် remote memory သို့ တိုက်ရိုက် ဖတ်/ရေး | CPU ကုန်ကျမှု နည်း |
| RoCE | RDMA over Converged Ethernet — Ethernet ပေါ်တွင် RDMA | |
| SHARP | Scalable Hierarchical Aggregation and Reduction Protocol — in-network computing (switch ထဲ တွင် reduction လုပ်) | NCCL နှင့် ပေါင်းသုံး |
| SM | Subnet Manager — IB subnet ကို စီမံပြီး LID/routes ခွဲဝေပေးသော component | ⚠️ GPU ရဲ့ SM (Streaming Multiprocessor) နှင့် မတူ |

### B.3 Hardware & System Management (၂၂ ခု)

| ဝေါဟာရ | အဓိပ္ပာယ် | မှတ်စု |
|---|---|---|
| BAR | Base Address Register — device memory ကို host address space ထဲ map ပေးသော PCIe register | GPU BAR1 = CPU မြင်ရသော GPU memory window |
| BMC | Baseboard Management Controller — out-of-band management controller | remote power, sensor, KVM (Module 07) |
| BOM | Bill of Materials — component စာရင်း | |
| DIMM | Dual In-line Memory Module — RAM module | |
| DMI | Desktop Management Interface — hardware description firmware table (SMBIOS/DMI) | `dmidecode` ဖတ်ရာ အရင်းအမြစ်; CPU context တွင် DMI = Direct Media Interface ဟုလည်း အဓိပ္ပာယ် ကွဲနိုင် |
| DPU | Data Processing Unit — networking/storage/security offload လုပ်သော programmable processor | BlueField (Module 13) |
| FRU | Field Replaceable Unit — လယ်ကွင်းတွင် လဲနိုင်သော component | GPU, PSU, DIMM... |
| IPMI | Intelligent Platform Management Interface — server management standard | `ipmitool` ဖြင့် အသုံးပြု |
| NUMA | Non-Uniform Memory Access — local/remote memory latency ကွဲသော multi-socket architecture | |
| NVMe | PCIe ပေါ် high-speed SSD interface | |
| OOM | Out Of Memory — memory ကုန်ခြင်း (kernel OOM killer / CUDA out of memory) | |
| PCIe | Peripheral Component Interconnect Express — expansion bus | GPU, NVMe ချိတ်ဆက်မှု |
| POST | Power-On Self Test — boot အချိန် firmware hardware check | |
| PSU | Power Supply Unit — စွမ်းအင်ပေးစက် | XID 54 နှင့် ဆက်စပ် |
| RMA | Return Merchandise Authorization — vendor ထံ hardware ပြန်လဲခြင်း လုပ်ငန်းစဉ် | Module 07 — "Mark for RMA" |
| SDR | Sensor Data Record — BMC sensor တစ်ခုချင်းကို ဖော်ပြသော IPMI record | `ipmitool sdr` |
| SEL | System Event Log — BMC ရဲ့ hardware event log | `ipmitool sel list` |
| SMBIOS | System Management BIOS — hardware ဖော်ပြသော firmware table standard | `dmidecode` ဖတ်သည် |
| SOL | Serial-over-LAN — serial console ကို network မှတစ်ဆင့် သုံးခြင်း | BMC/IPMI မှတစ်ဆင့် |
| SR-IOV | Single Root I/O Virtualization — physical device ကို virtual functions များ ခွဲခြင်း | VM/container network |
| UEFI | legacy BIOS အစား သုံးသော ခေတ်မီ firmware standard | BIOS verification mission နှင့် ဆက်စပ် |
| VRM | Voltage Regulator Module — PSU power ကို GPU/CPU လိုအပ်သော ဗို့အား သေးငယ်သို့ ပြောင်းပေး | |

### B.4 Cluster & Workload Management (၁၀ ခု)

| ဝေါဟာရ | အဓိပ္ပာယ် | မှတ်စု |
|---|---|---|
| BCM | Base Command Manager — NVIDIA ၏ cluster management software | provision/monitor/manage (Module 14) |
| GRES | Generic RESource — Slurm ၏ generic resource type | ဥပမာ `--gres=gpu:2` — GPU scheduling |
| HPC | High Performance Computing | |
| HPL | High Performance Linpack — sustained FP64 performance benchmark | D4 အဓိက test (Module 15) |
| MPI | Message Passing Interface — multi-process/multi-node parallel programming standard | `mpirun` |
| NCCL | NVIDIA Collective Communications Library — multi-GPU/node collective comms | all-reduce စသည် (Module 15) |
| NGC | NVIDIA GPU Cloud / NGC catalog — GPU-optimized container/model registry | `ngc` CLI (Module 14) |
| Slurm | open-source HPC job scheduler | `sinfo`, `sbatch`... (Module 08) |
| SPANK | Slurm Plug-in Architecture for Node and job (K)ontrol — job launch plugin framework | |
| UCX | Unified Communication X — RDMA/NVLink စသည့် high-perf communication framework | MPI/NCCL အောက်ခံ |

### B.5 Firmware & Tools (၆ ခု)

| ဝေါဟာရ | အဓိပ္ပာယ် | မှတ်စု |
|---|---|---|
| BER | Bit Error Rate — link signal quality တိုင်းတာသော ဘစ်အမှားနှုန်း | cable validation တွင် အရေးကြီး |
| FEC | Forward Error Correction — retransmission မပါဘဲ အချို့ ဘစ်အမှားများ ပြင်နိုင်အောင် redundancy ထည့်ခြင်း | HDR/NDR links |
| MFT | NVIDIA (Mellanox) Firmware Tools — NIC/IB firmware burn, query, debug tool suite | |
| MST | MFT အတွင်း device ကို low-level ချိတ်ဆက်ပေးသော component | `mst start`, `mst status` |
| PSID | Parameter Set ID — device NVMEM ထဲက firmware image ပါ board-specific firmware configuration ID | firmware မှန်/မမှန် စစ်ရာတွင် သုံး |
| PXE | Preboot Execution Environment — OS မတပ်ဆင်ရသေးမီ network မှ boot ခြင်း | node provisioning |

---

## Appendix C — Command Family Reference (17 categories / 214 commands)

dclabsim.com Docs > Commands တွင် command ၂၁၄ ခုကို category ၁၇ ခုဖြင့် ဖော်ပြထားပါသည်။
အောက်ပါဇယားတွင် တစ်ခုချင်း၏ command အရေအတွက်နှင့် **ဤ course တွင် သင်ကြားပြီးသော
အဓိက command များ** ကို ဖော်ပြထားပါသည် (★ = course တွင် တိုက်ရိုက် သုံးထား)။
*italic* = NVIDIA documentation တွင် ရှိသော်လည်း ဤ course တွင် ဥပမာအနေဖြင်သာ ဖော်ပြထားသော command များ။

| Category | Commands | ဤ course မှ အဓိက command များ | Module |
|---|---|---|---|
| GPU Management | 7 | ★ `nvidia-smi`, `nvidia-smi -q`, `-d ECC/TEMPERATURE/POWER`, `-pl`, `topo -m`, `-mig 1`, `mig -cgi/-lgi/-dgi` | 01, 04, 05, 13 |
| Diagnostics | 9 | ★ `dcgmi diag -r 1/2/3`, `dcgmi health -s/-g`, ★ `nvidia-bug-report.sh` | 02, 03, 15 |
| Networking & InfiniBand | 29 | ★ `ibstat` (link state, rate) — *`ibstatus`*, *`ibdiagnet`*, *`perfquery`* | 06 |
| Cluster Management | 18 | ★ `sinfo`, ★ `sbatch` (Pyxis `--container-image`), *`srun`*, *`squeue`*, *`sacct`*, *`scontrol`* | 08, 14 |
| Monitoring | 18 | ★ `dcgmi dmon -e 1001,1002,...`, ★ `nvsm show health`, ★ `nvsm show system` | 02 |
| System Information | 15 | ★ `dmidecode`, ★ `dmesg` (`| grep -i xid` / `sxid`) | 03, 07 |
| Containers | 8 | ★ `docker run --gpus all nvidia/cuda nvidia-smi`, ★ Enroot/Pyxis (`sbatch --container-image`) | 14 |
| Firmware | 5 | *MFT (`mst`)/`mlxup`-style firmware tools* — course တွင် procedure အနေဖြင့်သာ | 07 |
| Storage | 10 | course တွင် storage test အယူအဆ (Module 12/15) — command level မသင် | 12, 15 |
| GPU Fabric (NVLink/NVSwitch) | 5 | ★ `nvidia-smi nvlink -s/-e`, ★ `systemctl restart nvidia-fabricmanager` | 01, 06 |
| CUDA Tools | 12 | *`nvcc`* (illustrative — compile toolchain) | — |
| NCCL Tests | 8 | ★ `all_reduce_perf` (nccl-tests) + ★ `mpirun -np 8 ./all_reduce_perf -b 8 -e 256M -f 2` | 15 |
| MPI | 6 | ★ `mpirun -np ...` | 15 |
| RDMA Performance | 6 | *`ib_send_bw` / `ib_write_bw` (perftest family)* — bandwidth တိုင်းတာ | 06, 15 |
| Parallel Shell | 4 | simulator က single-node — ဥပမာအနေဖြင့်သာ (`pdsh`-style, illustrative) | — |
| Environment Modules | 1 | *`module load`* (illustrative) | — |
| General | 53 | ★ shell tool များ — `dmesg | grep`, `| head`, pipe/filter (Module 00) | 00, 03 |

### 214 vs ဤ course ဖုံးလွှမ်းမှု — နှိုင်းယှဉ်ချက်

- Platform က command **၂၁၄ ခု** အပြည့် ပါသော်လည်း စာမေးပွဲနှင့် လုပ်ငန်းခွင်တွင်
  လိုအပ်သည်မှာ **command ၂၁၄ ခု မှတ်မိခြင်း မဟုတ်** — မေးခွန်းတစ်ခု၏ အဖြေကို
  ပေးသော **tool family ကို ရွေးတတ်ခြင်း** ဖြစ်ပါသည် (Tool Selection quiz များကလည်း ဤပုံစံ)။
- ဤ course က core command ၃၀ ဝန်းကျင်ကို လက်တွေ့ သုံးစေပြီး ကျန် command များကို
  "ဘယ် family ထဲ ပါသလဲ" အဆင့်ဖြင့် မိတ်ဆက်ထားပါသည်။
- စာမေးပွဲတွင် အများဆုံး ပေါ်လာလေ့ရှိသော family များမှာ **GPU Management,
  Diagnostics, Networking & InfiniBand, Cluster Management, Monitoring** ဖြစ်ပြီး
  ဤ ၅ ခုကို ဦးစားပေး မှတ်ပါ (exam domain weight D4/D5 နှင့် ကိုက်ညီ)။

---

## 4. man page / `--help` output ဖတ်နည်း (အကျဉ်း)

```bash
man nvidia-smi        # အပြည့်အစုံ စာရွက်စာတမ်း
nvidia-smi --help     # option စာရင်း အမြန်ကြည့်
man -k gpu            # keyword ဖြင့် man page ရှာ
```

ဖတ်ပုံ အစဉ် — **NAME** (ဘာလဲ) → **SYNOPSIS** (syntax) → **DESCRIPTION** → **OPTIONS** → **EXAMPLES**။

- SYNOPSIS ထဲ `[ ]` = optional, `|` = ရွေးချယ်နိုင်, `...` = ထပ်ခါ ပြောနိုင်
- စာမေးပွဲတွင် man page မပါ — `--help` output တစ်ကွက် ပြပြီး "ဘယ် flag က ဘာလုပ်သလဲ"
  မေးနိုင်သဖြင့် option များကို ဖတ်ရှုနိုင်စွမ်း လေ့ကျင့်ထားပါ။
- man page ထဲက အချက်အလက်ကို မျက်စိမှိတ် မယုံပါနှင့် — version ကွာပါက behavior ကွဲနိုင်သည်
  (WRITING_RULES နှင့်လည်း ကိုက်ညီ — မသေချာပါက "needs verification")။

## Appendix D — Diagnostic Playbooks (4 scenarios)

dclabsim.com Docs > Troubleshooting မှ ဖြတ်လမ်း diagnostic playbooks ၄ ခု —
symptom ကနေ စစ်ဆေးရမည့် အဆင့် ၅ ဆင့် အထိ အကျဉ်းချုပ် (ပိုမို အသေးစိတ်ကို
Module 03/05/06/09 တွင် လေ့လာပါ)။

### Scenario A — XID Errors (GPU Faults)
**Symptoms:** application crash၊ 'GPU fallen off bus'၊ training နှေးခြင်း၊ NCCL timeout
1. `dmesg | grep -i nvrm` သို့မဟုတ် `journalctl -k | grep -i xid` — XID messages ရှာ
2. `nvidia-smi -q` ရဲ့ 'GPU Errors' section — ECC counts ကြည့်
3. `dcgmi diag -r 1` — quick GPU diagnostics
4. XID code ကို XID Reference (Appendix A) နှင့် တိုက်စစ်
5. `nvidia-smi -q -d TEMPERATURE` — thermal state စစ်

မှတ်ချက် — site ရဲ့ common-codes စာရင်းတွင် "XID 63 = Row Remapping Failure" ဟု
ဖော်ပြထားသော်လည်း XID Reference ကိုယ်တိုင်တွင် 63 = Success / 64 = Failure —
**63 = remap အောင်မြင် (Informational)** ဟု မှတ်ပါ (Module 03 နှင့် ကိုက်ညီ)။

### Scenario B — Thermal Throttling
**Symptoms:** clocks ကျ (SW Power Cap)၊ fan အသံကျယ်၊ performance ကျ၊ P-state ပြောင်း
1. `ipmitool sensor list | grep -i temp` — 'Inlet Temp' (< 35°C ဖြစ်သင့်)
2. `nvidia-smi -q -d TEMPERATURE` — GPU temps (warning > 83°C)
3. `nvidia-smi` ရဲ့ `Performance State` — P0=max၊ ကြီးလေ ကျလေ
4. `nvidia-smi -q -d CLOCK` — current vs max clocks
5. Airflow / vents / fans ရုပ်ပိုင်း စစ်

### Scenario C — NVLink Errors
**Symptoms:** GPU-to-GPU bandwidth ကျ၊ NCCL allreduce နှေး
1. `nvidia-smi nvlink -s` — link status ('Active' ဖြစ်သင့်)
2. `nvidia-smi nvlink -e` — error counts
3. `dcgmi nvlink -e` — detailed NVLink error counters
4. `nvidia-smi topo -m` — expected topology နှင့် တိုက်စစ်
5. ဆက်လက် error တက်ပါက GPU reseat သို့မဟုတ် cable လဲ

### Scenario D — InfiniBand Connectivity
**Symptoms:** network timeout၊ distributed training နှေး၊ NCCL init fail
1. `ibstat` — port state ('Active' ဖြစ်သင့်)
2. `ibstatus` — link width နှင့် speed
3. `ibdiagnet` — fabric-wide diagnostics
4. `perfquery` — port error counters
5. Cable connections / physical damage စစ်

## Appendix E — Official NVIDIA Study Resources

dclabsim.com Docs > Exam Guide မှ ညွှန်းထားသော တရားဝင် အရင်းအမြစ်များ
(စာမေးပွဲ အတွက် authoritative sources များ):

| အရင်းအမြစ် | ဘာအတွက် |
|---|---|
| NCP-AII Certification Page (nvidia.com) | certification overview, registration |
| NVIDIA Deep Learning Institute (DLI) | official training courses |
| DGX System Documentation | DGX A100/H100 user guides, admin manuals |
| Base Command Manager Docs | BCM install/config/admin |
| DCGM Documentation | GPU health/diagnostics/API |
| NVIDIA Driver Documentation | CUDA/driver compatibility |
| NVSM User Guide | fleet management |
| XID Error Reference (NVIDIA) | XID codes + troubleshooting |
| NVIDIA Networking (Mellanox) Docs | InfiniBand, ConnectX, BlueField |
| Slurm Workload Manager docs | config, GRES, GPU scheduling |
| NVIDIA GPU Operator docs | Kubernetes GPU operator |
| NCCL Documentation | multi-GPU collectives |
| MIG User Guide | MIG config/management |
| IPMI Guide | BMC management protocol |

## Self-Check

1. XID 76 က ဘာကို ဆိုလိုသလဲ၊ severity ဘာလဲ?
2. Critical severity XID တွေ့ပါက ပထမဆုံး ဘာလုပ်ရမလဲ (Module 03 စည်းမျဉ်း)?
3. GRES ဆိုတာ ဘာလဲ၊ ဘယ် tool နှင့် ဆက်စပ်သလဲ?
4. "multi-node all-reduce bandwidth တိုင်းချင်တယ်" — ဘယ် command family သုံးရမလဲ?
5. GPU context နှင့် IB context ထဲက SM ၂ မျိုး ကွာခြားချက်?

**အဖြေ** — 1) NVLink link training failed — Critical · 2) evidence ရယူ (`dmesg`, `nvidia-bug-report.sh`) ပြီးမှ remediate — ချက်ချင်း ဆောင်ရွက်ရန် · 3) Slurm ၏ Generic RESource — GPU scheduling · 4) NCCL Tests (`all_reduce_perf`) · 5) Streaming Multiprocessor (GPU compute unit) vs Subnet Manager (IB subnet စီမံ)

## Technical review notes

- Appendix A ရှိ ကုဒ် ၂၈ ခု၏ အဓိပ္ပာယ်နှင့် severity ခွဲခြားမှုသည် **dclabsim.com Docs
  (v1.6.1, 2026-10-14 capture)** အတိုင်း ဖြစ်ပြီး NVIDIA ၏ လက်တွေ့အပြုအမူကို
  ပညာရေးဆိုင်ရာ approximation အဖြစ် ဖော်ပြခြင်း ဖြစ်သည် — လက်တွေ့တွင်
  NVIDIA XID documentation ကို ကိုးကားပါ။ driver version အလိုက် အသေးစိတ်
  ကွဲနိုင်မှုကို အတည်မပြုနိုင်သေးပါ (**needs verification**)။
- NVIDIA ၏ တရားဝင် catalog တွင် XID အမျိုးအစားခွဲခြားမှု (application/driver/HW class)
  သည် dclabsim.com ၏ Critical/Warning/Informational ခွဲခြားမှုနှင့် မတူညီနိုင်ပါ —
  စာမေးပွဲအတွက် platform ၏ label ကို လိုက်နာပါ။
- Module 03 ဇယားတွင် ဖော်ပြထားသော XID 194 (power brake) သည် platform ၏ ၂၈-ကုဒ်
  reference တွင် မပါဝင်ပါ — Module 03 ၏ "needs verification" အတိုင်း ဆက်လက် သတ်မှတ်ထားပါသည်။
- Appendix C တွင် command တစ်ခုချင်းကို မည်သည့် platform category က ဖုံးလွှမ်းသည်ကို
  အတိအကျ မမှတ်တမ်းတင်နိုင်ပါ — category အလိုက် စုစုပေါင်း (7/9/29/.../53 = 214)
  မှာ dclabsim.com Docs capture အတိုင်း ဖြစ်ပြီး ဇယားထဲ ဆက်စပ်ဖော်ပြမှုများက
  course အကြောင်းအရာနှင့် ကိုက်ညီစွာ ရွေးချယ်ထားခြင်း ဖြစ်သည် (**needs verification** —
  platform ၏ တိကျသော command→category mapping)။
- `mlxup` / `mlxfwmanager` / MFT (`mst`) ကဲ့သို့ firmware tool အမည်များသည် platform version
  နှင့် NIC generation ပေါ် မူတည်၍ ကွဲနိုင်ပြီး simulator တွင် procedure အနေဖြင့်သာ
  ဖော်ပြထားပါသည် — တိကျသော command syntax **needs verification**။
- DMI ဝေါဟာရသည် context အလိုက် အဓိပ္ပာယ် ၂ မျိုး ရှိနိုင်သည်ကို Appendix B တွင် မှတ်သားထားပါသည်။
