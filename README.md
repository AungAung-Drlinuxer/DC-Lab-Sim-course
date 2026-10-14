# DC Lab Sim — NCP-AII Hands-on Lab Course

> Browser-based datacenter lab simulator **dclabsim.com** ကို reference အဖြစ် သုံးပြီး
> **NVIDIA-Certified Professional: AI Infrastructure (NCP-AII)** စာမေးပွဲအတွက်
> လက်တွေ့ command လေ့ကျင့်မှု + အသေးစိတ် ရှင်းလင်းချက်များ ပါဝင်သော လေ့လာရေး course။

## ဤ Course က ဘာလဲ

DC Lab Sim (github.com/Seanbo5386/dc-lab-sim) သည် **ရိုးရာ datacenter တစ်ခုလုံးကို browser ထဲတွင်
တုပဆောက်လုပ်ထားသော simulator** ဖြစ်ပြီး — DGX cluster တစ်ခု၏ terminal, telemetry dashboard,
fault injection, guided missions များကို **စက်အစစ် မလိုဘဲ** လေ့ကျင့်နိုင်ပါသည်။

- **40 guided missions** (200+ commands, story ပါ scenario များ) + **Live Incidents** (timed, scored)
- **6 architecture** — DGX A100 / H100 / H200 / B200 / GB200 / VR200
- **400+ practice questions** (exam domain weight အလိုက်)
- **Sandbox** — fault inject → diagnose → remediate (မှတ်မရှိ၊ စိတ်ကြိုက် စမ်းသပ်)
- **Simulated commands** — `nvidia-smi`, `dcgmi`, `nvsm`, `ibstat`, `sinfo`, `ipmitool`, `dmidecode`, `dmesg`

⚠️ DC Lab Sim သည် NVIDIA ၏ တရားဝင် ထုတ်ကုန် မဟုတ်ပါ — လေ့ကျင့်ရေး tool ဖြစ်ပြီး
တရားဝင် NVIDIA training/docs နှင့် ပေါင်းစပ် လေ့လာပါ။

## ဘာကြောင့် ဤ simulator လိုအပ်သလဲ

NCP-AII သည် **လက်တွေ့ လုပ်ဆောင်မှု (hands-on)** ကို မေးသော professional-level စာမေးပွဲ ဖြစ်ပါသည် —
"command ဘယ်ဟာလဲ" ထက် **"နောက်ဆုံး အဆင့်က ဘာလဲ / ဘာက ကြိုတင် ပြီးနေရမလဲ"** ကို ပိုမေးပါသည်။
DGX hardware အစစ် မကိုင်ဖူးသော ကျောင်းသား အများစုအတွက် ဤ simulator က command output
ဖတ်နည်း၊ fault ရှာနည်း၊ sequence အလိုက် ဖြေရှင်းနည်းကို လေ့ကျင့်ပေးပါသည်။

## NCP-AII စာမေးပွဲ အချက်အလက်

| အချက် | တန်ဖိုး |
|---|---|
| အမည် | NVIDIA-Certified Professional: AI Infrastructure |
| မေးခွန်း | 70-75 (multiple choice + multiple response) |
| အချိန် | 120 မိနစ် |
| ဈေးနှုန်း | $400 |
| Provider | Certiverse (remote proctored) |
| အောင်မှတ် | NVIDIA က မထုတ်ပြန် (70%+ မှန်းဆ လေ့ကျင့်) |
| သက်တမ်း | 2 နှစ် |
| အကြံပြု အတွေ့အကြုံ | datacenter လုပ်ငန်း 2-3 နှစ် (prerequisite မရှိ) |

**Domain ၅ ခု** — D1 Systems & Server Bring-Up **31%** · D2 Physical Layer **5%** ·
D3 Control Plane **19%** · D4 Cluster Test & Verification **33%** · D5 Troubleshooting & Optimization **12%**

📌 NCA-AIIO (associate) က အစိတ်အပိုင်းကို **မှတ်မိရုံ** မေးပါသည်၊ NCP-AII က **တပ်ဆင်ပြီး
အလုပ်လုပ်ကြောင်း သက်သေပြရန်** မေးပါသည် — ဤ course က ထိုကွာခြားချက်ကို ဖြည့်ပေးပါသည်။

## Course Structure (18 modules)

| # | Module | အကြောင်းအရာ | NCP-AII Domain |
|---|---|---|---|
| 00 | Orientation | DC Lab Sim လမ်းညွှန်၊ architecture များ၊ study method | — |
| 01 | nvidia-smi & Telemetry | GPU/node state ဖတ်နည်း | D4, D5 |
| 02 | DCGM & NVSM | dcgmi diag/dmon/health၊ cluster telemetry | D4 |
| 03 | XID Errors | XID/SXid ဖတ်၍ fault ရှာနည်း | D5 |
| 04 | ECC & Memory Errors | ECC correctable/uncorrectable စီမံခန့်ခွဲမှု | D5 |
| 05 | Thermal & Power | အပူ/စွမ်းအင် fault များ | D1.5, D5 |
| 06 | NVLink & InfiniBand | fabric topology နှင့် link fault | D4, D5 |
| 07 | BMC & Physical Repair | BMC/OOB/TPM, ipmitool, reseat/RMA | D1 |
| 08 | Slurm & Workloads | sinfo, workload အမျိုးအစား performance | D4 |
| 09 | Complex Scenarios | multi-symptom failure ဖြေရှင်းခြင်း | D5 |
| 10 | Missions Playbook | 40 missions strategy + command playbook | အားလုံး |
| 11 | Exam Readiness | blueprint map, trap များ, practice plan | အားလုံး |
| 12 | AI Factory Topology & Storage | topology, cabling, storage (knowledge) | D1, D4 |
| 13 | BlueField DPU & MIG | DPU offload, MIG partitioning (knowledge) | D1, D4 |
| 14 | Control Plane Stack | cluster software stack (knowledge) | D3 |
| 15 | Test & Verification Deep | acceptance testing deep (knowledge) | D4 |
| 16 | Mission-by-Mission Playbooks | မစ်ရှင် ၄၀ ခု တစ်ခုချင်း playbook | အားလုံး |
| 17 | Exam Modes & Live Incidents | exam modes ၅ မျိုး + Tool Mastery + Live Incidents | အားလုံး |
| 18 | Reference Appendices | XID 28 codes, glossary, command families | အားလုံး |

## အသုံးပြုပုံ

1. module တစ်ခုချင်းစီတွင် `explanation.md` → `exercise.md` → `solution.md` အတိုင်း လေ့လာပါ
2. လေ့ကျင့်ခန်း အားလုံးကို **https://www.dclabsim.com** ထဲတွင် လက်တွေ့ လုပ်ဆောင်ပါ
3. module ပြီးတိုင်း site ထဲက **domain quiz** ကို ဖြေပြီး အမှတ် တိုင်းတာပါ
4. အားလုံးပြီးမှ **missions 32 ခု** ကို အစဉ်လိုက် ဖြတ်ပါ (Module 10 playbook သုံး)

## လိုအပ်ချက်

- Desktop browser (1280px+) — site က desktop အတွက် အကောင်းဆုံး
- dclabsim.com အကောင့် (progress သိမ်းရန် — မသိမ်းလည်း လေ့ကျင့်လို့ရ)
- NCA-AIIO level အခြေခံ သိထားပါက ပိုမြန်ပါမည် (မရှိလည်း ရပါသည်)

## Reference များ

- DC Lab Sim: https://www.dclabsim.com
- Source: https://github.com/Seanbo5386/dc-lab-sim
- NCP-AII Exam: https://www.nvidia.com/en-us/learn/certification/ai-infrastructure-professional/
- NCP-AII Study Guide (PDF): NVIDIA-Certified Professional: AI Infrastructure Exam Study Guide
