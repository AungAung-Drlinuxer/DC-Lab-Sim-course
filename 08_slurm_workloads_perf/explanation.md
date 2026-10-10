# Module 08 — Slurm & Workload Simulation

## 1. ပြဿနာ — cluster မှာ workload ကို ဘယ်သူ စီမံမလဲ

GPU node များစွာ ရှိသော AI cluster တွင် user job များသည် တစ်ချိန်လုံး ဝင်လာနေပါသည်။
မည်သည့် job က မည်သည့် node ကို ရမည်၊ node တစ်ခု အလုပ်မလုပ်နိုင်တော့ပါက ဘယ်လို သိမည်၊
GPU utilization နည်းနေပါက GPU ပြဿနာလား workload ပြဿနာလား မည်သို့ ခွဲခြားမည် —
ဤမေးခွန်းများကို မဖြေရှင်းနိုင်ပါက cluster တစ်ခုလုံး စွမ်းဆောင်ရည် ကျသွားသော်လည်း
အကြောင်းရင်း အမှန်ကို မမြင်ဘဲ အချိန် ကုန်သွားနိုင်ပါသည်။

ဤ chapter သည် **job scheduler** အဖြစ် အသုံးများသော Slurm ၏ အခြေခံ အသုံးပြုပုံ၊
node state ဖတ်နည်း၊ workload အမျိုးအစား အလိုက် မျှော်လင့်ရမည့် telemetry ပုံစံနှင့်
utilization ကို အသုံးပြု၍ ပြဿနာ ခွဲခြားနည်းကို ဖုံးလွှမ်းပါသည်။

## 2. Slurm က ဘာလဲ

Slurm သည် HPC/AI cluster များတွင် **job scheduling + resource management** လုပ်သော
open-source workload manager ဖြစ်ပါသည် — DGX cluster များတွင် အသုံးများဆုံး ဖြစ်ပါသည်။

Slurm က လုပ်ပေးသော အလုပ် အဓိက ၃ ခု —

- **Job queue စီမံခြင်း** — user တင်လိုက်သော job များကို queue ထဲ ထားပြီး resource လွတ်မှ
  node သို့ ချထားပေးခြင်း
- **Resource allocation** — node, GPU, CPU, memory ကို job တစ်ခုချင်းစီသို့ သတ်မှတ်ပေးခြင်း
- **Node state စီမံခြင်း** — node တစ်ခု အသုံးပြုနိုင်/မနိုင်ကို state ဖြင့် ဖော်ပြခြင်း

## 3. sinfo — node အခြေအနေ စစ်ဆေးခြင်း ⭐

```bash
sinfo        # partition အလိုက် node state များ၏ အနေအထား စာရင်း ပြသသည်
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

စာမေးပွဲတွင် — "node က job အသစ် မလက်ခံနိုင်" ဆိုလျှင် `down`/`drain` state ကို ရှာပါ။

## 4. Workload ၄ မျိုး — expected output ကွာခြားချက် ⭐

Workload အမျိုးအစား ပေါ် မူတည်၍ GPU ၏ telemetry ပုံစံသည် ကွဲပြားပါသည် —
ဤကွာခြားချက်ကို သိမှ "ဤ output သည် ဤ workload အတွက် ပုံမှန်လား မပုံမှန်လား" ခွဲခြားနိုင်မည်။
DC Lab Sim ၏ **Simulate Workloads** ဖြင့် လေ့ကျင့်နိုင်ပါသည် —

| Workload | Util | Memory | Power | Temp |
|---|---|---|---|---|
| Idle | ~5% | ~50-200MB | ~15% TDP | ambient နီး |
| Inference | ~60% | အလယ် | အလယ် | 40-60°C |
| Training | ~95% | မြင့် | မြင့် | 60-80°C |
| Stress Test | 100% | အမြင့် | cap အထိ | 70-85°C |

ဤဇယားရှိ တန်ဖိုးများသည် **ခန့်မှန်း အကွာအဝေး** များသာ ဖြစ်ပါသည် —
GPU model, cooling design, power cap, workload ၏ အမှန်တကယ် သဘောသဘာဝပေါ် မူတည်၍ ပြောင်းလဲနိုင်ပါသည်။

## 5. Utilization ဖတ်၍ ပြဿနာ ခွဲခြားခြင်း ⭐

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
အထက်ပါ အဆင့်များကို အပေါ်မှ အောက်သို့ တစ်ခုချင်း ဖြတ်သန်း စစ်ဆေးပါ။

## 6. ဘယ် workload က ဘာကို စစ်ပေးသလဲ ⭐ (tool တစ်ခုချင်း)

| Tool/Test | ဘာကို သက်သေပြသလဲ |
|---|---|
| Stress test | raw compute + cooling (တည်ငြိမ်မှု) |
| HPL (Linpack) | sustained FP64 compute + thermal |
| NCCL | GPU ကြား/node ကြား collective bandwidth |
| ClusterKit | node-to-node assessment |
| NeMo burn-in | end-to-end training path + storage/data |

Tool တစ်ခုချင်းစီသည် သက်သေပြပေးသော အချက် မတူပါ —
ဘာကို စစ်ချင်သလဲ ဆိုသည်နှင့် ကိုက်ညီသော tool ကို ရွေးပါ။

## 7. လုပ်ငန်းခွင် သတိပြုရန် အချက်များ

- **util နည်း ≠ GPU ပျက်** — ပထမဦးစွာ workload ရှိ/မရှိ (sinfo) နှင့် data/CPU path ကို
  ဖြတ်ပြီးမှ GPU fault (XID/ECC) ဘက်သို့ သွားပါ။
- **ဇယားတန်ဖိုးများကို စည်းမျဉ်း မထားပါနှင့်** — §4 ရှိ temp/power/util အကွာအဝေးများသည်
  ခန့်မှန်းချက် ဖြစ်ပြီး GPU model နှင့် cooling ပေါ် မူတည်ပါသည် — ကိုယ်ပိုင် cluster ၏
  baseline ကို ကိုယ်တိုင် တိုင်းတာ မှတ်တမ်းတင်ပါ။
- **node state ကို partition အလိုက် ကြည့်ပါ** — `sinfo` သည် partition အလိုက် ပြသသောကြောင့်
  node တစ်ခုသည် partition တစ်ခုတွင် alloc ဖြစ်နေပြီး အခြားတွင် မတူညီသော state မြင်ရနိုင်ပါသည်။
- **`drain`/`down` node တွင် job တင်ထားရုံဖြင့် အလုပ်မဖြစ်ပါ** — admin ၏ ပြုပြင်မှု
  (ပြန်ဖွင့်ခြင်း/ပြုပြင်ခြင်း) ပြီးမှ node သည် job အသစ် ပြန်လက်ခံပါမည်။

## 8. Self-Check

1. `sinfo` ထဲက `drain` state ဆိုတာ ဘာကို ဆိုလိုသလဲ?
2. Training workload တွင် မျှော်လင့်ရမည့် utilization က မည်မျှလဲ?
3. GPU util နည်းနေလျှင် စစ်ဆေးရန် အကြောင်းရင်း ၃ ခု ဖော်ပြပါ။
4. NCCL test က ဘာကို သက်သေပြသလဲ?

**အဖြေ** —
1) maintenance ပြုလုပ်နေသော node — job အသစ် မလက်ခံပါ။
2) ခန့်မှန်းချက်အားဖြင့် ~95% (GPU model ပေါ် မူတည်)။
3) workload ရှိ/မရှိ (sinfo)၊ data pipeline/CPU bottleneck၊ power/thermal throttle သို့မဟုတ် GPU fault (XID/ECC)။
4) GPU ကြား/node ကြား collective bandwidth။

## 9. References

- Slurm: https://slurm.schedmd.com/
- DGX best practices: https://docs.nvidia.com/datacenter/dgx/

## Technical review notes

- §4 ရှိ workload expected-output ဇယား တန်ဖိုးများ (util/memory/power/temp) သည် ခန့်မှန်း
  အကွာအဝေးများဖြစ်ပြီး GPU model, driver, cooling, power cap ပေါ် မူတည် — needs verification
  (စာသားတွင် ဖော်ပြပြီး ဖြစ်ပါသည်)။
- Training workload အတွက် ~95% utilization ဆိုသော အချက်သည် ယေဘုယျ ခန့်မှန်းချက်ဖြစ်ပြီး
  input pipeline ပေါ် မူတည်၍ ပြောင်းလဲနိုင် — needs verification။
- အခြား နည်းပညာ အကြောင်းအရာ (Slurm node states, tool အသုံးပြုပုံ) များကို မူရင်း အဓိပ္ပာယ်
  အတိုင်း ထိန်းသိမ်းထားပါသည် — ပြင်ဆင်မှု မပြုလုပ်ပါ။
