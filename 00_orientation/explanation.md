# Module 00 — DC Lab Sim Orientation

## 1. ဘာကြောင့် ဒီ module လိုအပ်သလဲ

Datacenter GPU operations တွင် fault တစ်ခု ဖြစ်လာပါက တကယ့် hardware ကို မထိဘဲ
လေ့ကျင့်ရန် ခက်ခဲပါသည် — production cluster တွင် fault တမင် ဖန်တီး၍ မရပါ။
**DC Lab Sim** က ဤပြဿနာကို ဖြေရှင်းပေးပါသည် — browser ထဲတွင် DGX cluster တစ်ခုလုံးကို
တုပဆောက်လုပ်ထားပြီး fault inject, diagnose, remediate ကို အန္တရာယ်ကင်းစွာ
လေ့ကျင့်နိုင်ပါသည်။

ဤ module သည် simulator ၏ structure, mode များနှင့် command family များကို မိတ်ဆက်ပေးပြီး
နောက် module အားလုံးအတွက် အခြေခံ ဖြစ်စေပါသည်။

## 2. DC Lab Sim က ဘာလဲ

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

- **Dashboard** — cluster အဆင့် အခြေအနေကို အမြင်အားဖြင့် မြန်မြန် ကြည့်ရှုရာ
- **Terminal** — တကယ့် datacenter တွင် သုံးသကဲ့သို့ command များ ရိုက်ထည့် လုပ်ဆောင်ရာ
- **Sandbox / Missions / Quizzes** — လေ့ကျင့်ရေး mode ၃ မျိုး (အောက်တွင် နှိုင်းယှဉ်ထားပါသည်)

## 3. Architecture ၆ မျိုး — spec ပြောင်းလျှင် ဘာတွေ ပြောင်းလဲ

| System | GPUs | Memory | NVLink | Network |
|---|---|---|---|---|
| DGX A100 | 8× A100 | 80GB HBM2e | 3rd gen (12 links) | HDR 200Gb/s |
| DGX H100 | 8× H100 SXM | 80GB HBM3 | 4th gen (18 links) | NDR 400Gb/s |
| DGX H200 | 8× H200 SXM | 141GB HBM3e | 4th gen (18 links) | NDR 400Gb/s |
| DGX B200 | 8× B200 | 180GB HBM3e | 5th gen (18 links) | NDR 400Gb/s |
| DGX GB200 | 8× GB200 | 192GB HBM3e | 5th gen (18 links) | XDR 800Gb/s |
| DGX VR200 | 8× R200 | 288GB HBM4 | 6th gen (18 links) | XDR 800Gb/s |

⚠️ **စာမေးပွဲ trap** — procedure တစ်ခုချင်းစီက platform နှင့် ဆက်စပ်ပါသည် —
DGX A100 bring-up ဆိုတာ HGX B200 နှင့် မတူပါ။ Architecture dropdown ကို ပြောင်းလျှင်
GPU spec, NVLink topology, InfiniBand rate အားလုံး ပြောင်းလဲသွားပါမည်။

Memory capacity များသည် configuration ပေါ်လည်း မူတည်နိုင်ပါသည် (ဥပမာ — DGX A100 တွင်
40GB variant လည်း ရှိဖူးပါသည်)။ စာမေးပွဲတွင် ပေးထားသော platform spec ကို
အခြေခံ၍ ဖြေပါ။

## 4. Mode ၃ မျိုး — ဘယ်အခါ ဘာသုံး

| Mode | လုပ်ဆောင်ပုံ | Scoring | အသုံးပြုသင့်သော အခြေအနေ |
|---|---|---|---|
| **Sandbox** | fault ထည့် → terminal ဖြင့် ရှာ → ပြင် | မှတ်မရှိ | စိတ်ကြိုက် စမ်းသပ်၊ တစ်ခုချင်း ကျွမ်းကျင်အောင် လေ့ကျင့်ရာ |
| **Missions** | story ပါ guided scenario (32 ခု) — fault automatic inject, inline quiz + debrief ပါ | ရှိ | လုပ်ငန်းစဉ် တစ်ခုလုံး အစီအစဉ်ရှိရှိ ဖြတ်သန်း လေ့ကျင့်ရာ |
| **Quizzes** | domain weight အလိုက် 400+ မေးခွန်း | ရှိ | စာမေးပွဲ အဆင့်သင့်မှု စစ်ဆေးရာ |

Mission ဥပမာ — *"The Midnight Deployment"* — cluster အသစ် ည ၁၁ နာရီ ရောက်၊ မနက် ၈ နာရီ
production-ready ဖြစ်ရမည်။ `nvidia-smi` က GPU 6 တွင် `ERR!` ပြနေ — မှတ်တမ်းတင်၊ reset,
တစ် cluster လုံး validate။

## 5. Terminal လေ့ကျင့်နည်း — Command Family များ

မေးခွန်းတစ်ခုချင်းစီအတွက် သင့်လျော်သော command family ကို ရွေးချယ်တတ်ရန် အဓိက ဖြစ်ပါသည် —

```
မေးခွန်း: "GPU က ကျန်းမာလား?"         → nvidia-smi / dcgmi diag
မေးခွန်း: "memory error ရှိလား?"          → nvidia-smi -q -d ECC
မေးခွန်း: "အပူ များနေလား?"               → nvidia-smi -q -d TEMPERATURE / ipmitool sensor list
မေးခွန်း: "link ပျက်နေလား?"               → nvidia-smi nvlink -s / ibstat
မေးခွန်း: "node က job ယူနိုင်လား?"         → sinfo
မေးခွန်း: "kernel မှာ ဘာဖြစ်ခဲ့လဲ?"         → dmesg | grep -i xid
```

- `nvidia-smi` / `dcgmi` — GPU state နှင့် diagnostics (Module 01–02)
- `ipmitool` / `dmidecode` — BMC/hardware level (Module 07)
- `ibstat` — InfiniBand link state (Module 06)
- `sinfo` — Slurm ဖြင့် node availability (Module 08)
- `dmesg` — kernel log အတွင်း XID/SXid event ရှာရာ (Module 03)

## 6. လေ့လာနည်း Loop (module တိုင်း သုံးပါ)

```
learn (explanation) → inject (sandbox fault) → diagnose (commands)
      ↑                                            ↓
      └────────── verify (dashboard ပြန်ကြည့်) ← remediate
```

ဤ loop ကို module တိုင်းတွင် ပြန်လည် အသုံးပြုပါ — ဖတ်ပြီးတိုင်း sandbox တွင် fault ထည့်ကြည့်ခြင်းက
တကယ့်အခြေအနေတွင် ပြန်သုံးနိုင်မည့် အမှတ်ရမှုကို အားကောင်းစေပါသည်။

## 7. လုပ်ငန်းခွင် သတိပြုရန် အချက်များ

- Simulator ထဲက spec များနှင့် output များက တကယ့် environment တွင် GPU model, driver version,
  tool version ပေါ် မူတည်၍ ကွဲပြားနိုင်ပါသည် — တိကျသော ဂဏန်းများကို မှတ်မိရုံမဟုတ်ဘဲ
  ဖတ်ပုံ နည်းလမ်းကို သင်ယူပါ။
- Dashboard နှင့် terminal ၂ ခုလုံး ကြည့်ပါ — တကယ့် datacenter တွင် monitoring system နှင့် SSH
  ၂ မျိုးလုံးကို အသုံးပြုရသကဲ့သို့ ဖြစ်ပါသည်။
- Mission တွင် scoring ရှိသောကြောင့် ဖြေရှင်းမှု မလုပ်မီ မှတ်တမ်းတင်ခြင်း (document) ကို
  အလေ့အကျင့် လုပ်ထားပါ။

## 8. Self-Check

1. Sandbox နှင့် Mission ကွာခြားချက် ၃ ချက် ပြောပါ။
2. DGX H100 တွင် NVLink link အရေအတွက် မည်မျှရှိသနည်း။
3. "GPU 6 တွင် ERR!" ဆိုပါက ပထမဆုံး run သင့်သော command?
4. "node က job ယူနိုင်/မယူနိုင်" ကို ဘယ် command နှင့် စစ်သနည်း။
5. Architecture dropdown ကို ပြောင်းလျှင် ဘာတွေ ပြောင်းလဲသွားသနည်း။

**အဖြေ** — 1) scoring (no/yes), fault injection (manual/automatic), structure (free/guided+quiz+debrief) · 2) 18 links (4th gen) · 3) `nvidia-smi` (full state ကြည့်) · 4) `sinfo` · 5) GPU spec, NVLink topology, InfiniBand rate အားလုံး

## 9. References

- https://www.dclabsim.com
- https://github.com/Seanbo5386/dc-lab-sim

## Technical review notes

- DGX B200 memory ကို မူရင်းပါ 192GB အစား **180GB HBM3e** ဟု ပြင်ဆင်ခဲ့သည် — NVIDIA ရုံးတရားဝင်
  DGX/HGX B200 spec sheet များအရ တစ် GPU လျှင် 180GB (စုစုပေါင်း 1,440GB) ဖြစ်ပြီး 192GB သည်
  physical HBM3e stack size (software-visible မဟုတ်) ဖြစ်သည်။
- DGX GB200 (8× GB200, 192GB HBM3e, XDR 800Gb/s) နှင့် DGX VR200 (8× R200, 288GB HBM4,
  NVLink 6th gen, XDR 800Gb/s) spec များက model generation နှင့် roadmap ပေါ် မူတည် —
  **needs verification** (NVIDIA official DGX documentation ဖြင့် တိုက်စစ်ပါ)။
- Memory capacity များ (80GB/141GB/180GB) မှာ DGX configuration ပေါ် မူတည်နိုင် —
  **needs verification** (စာမေးပွဲတွင် ပေးသော spec ကိုသာ အခြေခံပါ)။
