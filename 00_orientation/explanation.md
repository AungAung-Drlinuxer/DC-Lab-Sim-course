# Module 00 — DC Lab Sim Orientation

## 1. DC Lab Sim က ဘာလဲ

**DC Lab Sim** (dclabsim.com) သည် browser ထဲတွင် datacenter တစ်ခုကို တုပဆောက်လုပ်ထားသော simulator ဖြစ်ပါသည် —
DGX cluster တစ်ခုတွင် တွေ့ရမည့် terminal, GPU telemetry dashboard, fault များကို အစစ်မဟုတ်သော်လည်း
ယုတ္တိဗေဒ မှန်ကန်စွာ ဖြင့် လေ့ကျင့်နိုင်ပါသည်။

```
┌────────────────────────────────────────────────────────────┐
│                    DC Lab Sim (browser)                    │
├──────────────┬─────────────────────────────────────────────┤
│ Dashboard    │  Cluster Health: 8/8 nodes, 64/64 GPUs      │
│ (telemetry)  │  InfiniBand ✓ · BCM HA ✓ · NVLink topology  │
├──────────────┼─────────────────────────────────────────────┤
│ Terminal     │  nvidia-smi · dcgmi · nvsm · ibstat         │
│ (simulated)  │  sinfo · ipmitool · dmidecode · dmesg       │
├──────────────┼─────────────────────────────────────────────┤
│ Sandbox      │  fault inject → diagnose → remediate (no score)│
├──────────────┼─────────────────────────────────────────────┤
│ Missions     │  32 guided scenarios, 229 commands (scored) │
├──────────────┼─────────────────────────────────────────────┤
│ Quizzes      │  400+ questions (exam domain weight အလိုက်) │
└──────────────┴─────────────────────────────────────────────┘
```

## 2. Architecture ၆ မျိုး — spec ပြောင်းလျှင် ဘာတွေ ပြောင်းလဲ

| System | GPUs | Memory | NVLink | Network |
|---|---|---|---|---|
| DGX A100 | 8× A100 | 80GB HBM2e | 3rd gen (12 links) | HDR 200Gb/s |
| DGX H100 | 8× H100 SXM | 80GB HBM3 | 4th gen (18 links) | NDR 400Gb/s |
| DGX H200 | 8× H200 SXM | 141GB HBM3e | 4th gen (18 links) | NDR 400Gb/s |
| DGX B200 | 8× B200 | 192GB HBM3e | 5th gen (18 links) | NDR 400Gb/s |
| DGX GB200 | 8× GB200 | 192GB HBM3e | 5th gen (18 links) | XDR 800Gb/s |
| DGX VR200 | 8× R200 | 288GB HBM4 | 6th gen (18 links) | XDR 800Gb/s |

⚠️ **စာမေးပွဲ trap** — procedure တစ်ခုချင်းစီက platform နှင့် ဆက်စပ်ပါသည် —
DGX A100 bring-up ဆိုတာ HGX B200 နှင့် မတူပါ။ Architecture dropdown ကို ပြောင်းလျှင်
GPU spec, NVLink topology, InfiniBand rate အားလုံး ပြောင်းလဲသွားပါမည်။

## 3. Mode ၃ မျိုး — ဘယ်အခါ ဘာသုံး

1. **Sandbox** — fault ထည့် → terminal ဖြင့် ရှာ → ပြင်။ မှတ်မရှိ။ စိတ်ကြိုက် စမ်းသပ်ရာ
2. **Missions** — story ပါ guided scenario (32 ခု)။ fault ကို automatic inject လုပ်ပေးပြီး inline quiz + debrief ပါ
3. **Quizzes** — domain weight အလိုက် 400+ မေးခွန်း

Mission ဥပမာ — *"The Midnight Deployment"* — cluster အသစ် ည ၁၁ နာရီ ရောက်၊ မနက် ၈ နာရီ
production-ready ဖြစ်ရမည်။ `nvidia-smi` က GPU 6 တွင် `ERR!` ပြနေ — မှတ်တမ်းတင်၊ reset,
တစ် cluster လုံး validate။

## 4. Terminal လေ့ကျင့်နည်း — Command Family များ

```
မေးခွန်း: "GPU က ကျန်းမာလား?"         → nvidia-smi / dcgmi diag
မေးခွန်း: "memory error ရှိလား?"          → nvidia-smi -q -d ECC
မေးခွန်း: "အပူ များနေလား?"               → nvidia-smi -q -d TEMPERATURE / ipmitool sensor list
မေးခွန်း: "link ပျက်နေလား?"               → nvidia-smi nvlink -s / ibstat
မေးခွန်း: "node က job ယူနိုင်လား?"         → sinfo
မေးခွန်း: "kernel မှာ ဘာဖြစ်ခဲ့လဲ?"         → dmesg | grep -i xid
```

## 5. လေ့လာနည်း Loop (module တိုင်း သုံးပါ)

```
learn (explanation) → inject (sandbox fault) → diagnose (commands)
      ↑                                            ↓
      └────────── verify (dashboard ပြန်ကြည့်) ← remediate
```

## 6. Self-Check

1. Sandbox နှင့် Mission ကွာခြားချက် ၃ ချက် ပြောပါ။
2. DGX H100 တွင် NVLink link အရေအတွက် မည်မျှရှိသနည်း။
3. "GPU 6 တွင် ERR!" ဆိုပါက ပထမဆုံး run သင့်သော command?

**အဖြေ** — 1) scoring (no/yes), fault injection (manual/automatic), structure (free/guided+quiz+debrief) · 2) 18 links (4th gen) · 3) `nvidia-smi` (full state ကြည့်)

## 7. References

- https://www.dclabsim.com
- https://github.com/Seanbo5386/dc-lab-sim
