# Module 08 — Slurm & Workload Simulation

## 1. Slurm က ဘာလဲ

Slurm သည် HPC/AI cluster များတွင် **job scheduling + resource management** လုပ်သော
open-source workload manager ဖြစ်ပါသည် — DGX cluster များတွင် အသုံးများဆုံး ဖြစ်ပါသည်။

## 2. sinfo — node အခြေအနေ ⭐

```bash
sinfo        # partition/node state စာရင်း
```

Node state အဓိက များ —
| State | အဓိပ္ပာယ် |
|---|---|
| **idle** | job မရှိ — လက်ခံနိုင် |
| **alloc** (allocated) | job က သုံးနေ |
| **down** | admin ပိတ်ထား / ပျက်နေ |
| **drain** | maintenance — job အသစ် မလက်ခံ |
| **mix** | အချို့ resource သုံးနေ၊ အချို့ လွတ် |
| **fail** | health check ကျဆင်း |

စာမေးပွဲတွင် — "node က job အသစ် မလက်ခံနိုင်" ဆိုလျှင် `down/drain` state ရှာပါ။

## 3. Workload ၄ မျိုး — expected output ကွာခြားချက် ⭐

DC Lab Sim ၏ **Simulate Workloads** ဖြင့် လေ့ကျင့်နိုင်ပါသည် —

| Workload | Util | Memory | Power | Temp |
|---|---|---|---|---|
| Idle | ~5% | ~50-200MB | ~15% TDP | ambient နီး |
| Inference | ~60% | အလယ် | အလယ် | 40-60°C |
| Training | ~95% | မြင့် | မြင့် | 60-80°C |
| Stress Test | 100% | အမြင့် | cap အထိ | 70-85°C |

## 4. Utilization ဖတ်၍ ပြဿနာ ခွဲခြားခြင်း ⭐

```
GPU util နည်းနေတယ် ဆိုပါက —
├─ workload ရှိ/မရှိ (sinfo — alloc လား idle လား)
├─ data ကျွေးနိုင်လား (input pipeline)
├─ CPU bottleneck (preprocessing)
├─ power/thermal throttle (Perf state)
└─ GPU fault (XID/ECC)
```

⚠️ လူအများ မှားတတ်သော အချက် — GPU util နည်းခြင်းသည် **GPU ပြဿနာ အမြဲ မဟုတ်**ပါ —
data loading / CPU ကြောင့် GPU က စောင့်နေခြင်းလည်း ဖြစ်နိုင်ပါသည်။

## 5. ဘယ် workload က ဘာကို စစ်ပေးသလဲ ⭐ (tool တစ်ခုချင်း)

| Tool/Test | ဘာကို သက်သေပြသလဲ |
|---|---|
| Stress test | raw compute + cooling (တည်ငြိမ်မှု) |
| HPL (Linpack) | sustained FP64 compute + thermal |
| NCCL | GPU ကြား/node ကြား collective bandwidth |
| ClusterKit | node-to-node assessment |
| NeMo burn-in | end-to-end training path + storage/data |

## 6. Self-Check

1. `sinfo` ထဲက `drain` state ဆိုတာ?
2. Training workload တွင် မျှော်လင့်ရမည့် utilization?
3. GPU util နည်းနေလျှင် စစ်ဆေးရန် အကြောင်းရင်း ၃ ခု?

**အဖြေ** — 1) maintenance — job အသစ် မလက်ခံ · 2) ~95% · 3) workload ရှိ/မရှိ, data pipeline/CPU, throttle/fault (၃ ခု ရွေးရေး)

## 7. References

- Slurm: https://slurm.schedmd.com/
- DGX best practices: https://docs.nvidia.com/datacenter/dgx/
