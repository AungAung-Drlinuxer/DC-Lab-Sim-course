# Module 01 — nvidia-smi & GPU Telemetry

## 1. ဘာကြောင့် ဒီကျွမ်းကျင်မှု လိုအပ်သလဲ

"GPU က ကျန်းမာလား", "workload ပြေးနေလား", "အပူ/စွမ်းအင် ပြဿနာ ရှိလား" ဆိုသော
မေးခွန်းများက datacenter operation တွင် နေ့စဉ် ကြုံရပါသည်။ ထိုမေးခွန်းများအတွက်
ပထမဆုံး ကြည့်ရမည့် tool မှာ `nvidia-smi` ဖြစ်ပြီး ၎င်း၏ output ကို မှန်ကန်စွာ
ဖတ်တတ်ခြင်းသည် စာမေးပွဲနှင့် လက်တွေ့ ၂ ခုလုံးတွင် အမြင့်ဆုံး တန်ဖိုးရှိပါသည်။

ဤ chapter တွင် `nvidia-smi` output ဖတ်နည်း၊ utilization အဓိပ္ပာယ်ဖော်နည်း၊
query options များနှင့် topology ဖတ်နည်းကို လေ့လာပါမည်။

## 2. nvidia-smi က ဘာလဲ

`nvidia-smi` (NVIDIA System Management Interface) သည် GPU ၏ **state တစ်ခုလုံး** ကို
တစ်နေရာတည်းတွင် ပြသော tool ဖြစ်ပါသည် — device state, memory, temperature, power,
ECC, processes, topology အားလုံး။ စာမေးပွဲတွင် output ဖတ်ရှုရေး ကို အများဆုံး မေးပါသည်။

## 3. Default Output ဖတ်နည်း ⭐

```
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 535.129.03   Driver Version: 535.129.03   CUDA Version: 12.2     |
+-----------------------------------------------------------------------------+
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|===============================+======================+======================|
|   0  A100-SXM4-80GB      On   | 00000000:07:00.0 Off |                    0 |
| N/A   37C   P0    311W / 400W |   1234MiB / 81920MiB |      4%      Default |
+-----------------------------------------------------------------------------+
```

**Column တစ်ခုချင်း —**
- **Persistence-M** — `On` ဆိုလျှင် driver က GPU init ပြီး၊ process တိုင်း မလိုဘဲ state ဆက်ထား (datacenter တွင် `On` ဖြစ်သင့်)
- **Bus-Id** — PCIe address (fault report တွင် အသုံးပြု)
- **Volatile Uncorr. ECC** — uncorrectable ECC error အရေအတွက် (မသုည ဆိုလျှင် ပြဿနာ — Module 04)
- **Temp** — အပူချိန် (°C) — 35-75°C ပုံမှန်၊ 90°C+ ဆိုလျှင် thermal ပြဿနာ
- **Perf** — performance state: `P0` အပြည့်၊ `P2/P3` ဆိုလျှင် throttling ဖြစ်နေ
- **Pwr:Usage/Cap** — လက်ရှိ သုံးနေသော စွမ်းအင် / အမြင့်ဆုံး — cap ကို ကျော်နေလျှင် power ပြဿနာ
- **Memory-Usage** — အသုံးပြုပြီး/စုစုပေါင်း
- **GPU-Util** — compute utilization (စက္ကန့် ၁ အတွင်း kernel ကြောင်း ရာခိုင်နှုန်း)
- **Compute M.** — compute mode (Default/Exclusive/Prohibited)

ဖတ်ပုံ အဆင့်စဉ် — ပထမ header (driver/CUDA version) → GPU state အကြမ်း (ECC, util) →
နောက်တန်း column များ (temp, power, memory) ဟု အပေါ်မှ အောက်သို့ ဖတ်ပါ။

## 4. Utilization ဖတ်ခြင်း — ၃ ပုံစံ ⭐

| အခြေအနေ | အဓိပ္ပာယ် |
|---|---|
| Util 95-100% + Mem မြင့် | training workload ပုံမှန် ဖြစ်နေ |
| Util 0% + Mem 50-200MB | idle (workload မရှိ) — ပုံမှန် |
| Util 0% + Mem ပြည့်နေ | hang / memory leak — ပြဿနာ |
| Util မမြင့် + power cap ကျော် | power ပြဿနာ |

⚠️ "GPU util နည်းနေ" ဆိုတာ အမြဲ fault မဟုတ်ပါ — workload ရပ်နေခြင်းလည်း ဖြစ်နိုင်ပါသည်။
Util ဂဏန်းတစ်ခုတည်းကို မကြည့်ဘဲ memory, processes, dashboard နှင့် ပေါင်း၍ ကောက်ချက်ချပါ။

## 5. Query Options အရေးကြီးသောများ ⭐

```bash
nvidia-smi -q                      # full detail query (state အပြည့်)
nvidia-smi -q -d ECC               # ECC error အသေးစိတ်
nvidia-smi -q -d TEMPERATURE       # အပူချိန် (GPU/memory)
nvidia-smi -q -d POWER             # power draw/cap/limit
nvidia-smi topo -m                 # GPU topology (NVLink/PCIe ဆက်သွယ်မှု)
nvidia-smi nvlink -s               # NVLink status (link အလိုက် state)
nvidia-smi nvlink -e               # NVLink error counters
nvidia-smi dmon                    # dynamic monitoring (refresh)
nvidia-smi --query-gpu=index,temperature.gpu,power.draw --format=csv
```

- `-q` — ပုံမှန် table ထက် ပို၍ အသေးစိတ် ကြည့်ရှုရာ။ `-d ECC` ကဲ့သို့ ဘာသာရပ်ခွဲ စစ်ဆေးနိုင်
- `topo -m` / `nvlink -s` / `nvlink -e` — GPU များ အချင်းချင်း ချိတ်ဆက်မှုနှင့် link ကျန်းမာရေး (Module 06 သို့)
- `dmon` — refresh ဖြင့် ဆက်တိုက် စောင့်ကြည့်ရာ (live monitoring)
- `--query-gpu=... --format=csv` — script/monitoring ထဲ ထည့်သုံးနိုင်ရန် ရွေးချယ်၍ ထုတ်ယူရာ

## 6. Topology (`topo -m`) ဖတ်နည်း

```
        GPU0  GPU1  GPU2  GPU3
GPU0     X    NV18  NV18  NV18
GPU1    NV18   X    NV18  NV18
```
- **NV18/NV12** = NVLink (ဂဏန်း = link အရေအတွက်) — bandwidth အမြင့်ဆုံး
- **PIX/PIX** = same PCIe switch — သာမန်
- **SYS** = system (NUMA ကျော်) — အနှေးဆုံး
- training အတွက် NVLink အုပ်စု အတွင်း ထားခြင်းက အကောင်းဆုံး

## 7. Dashboard နှင့် နှိုင်းယှဉ် ဖတ်ခြင်း

DC Lab Sim တွင် dashboard (visual) နှင့် terminal (`nvidia-smi`) ၂ ခုလုံး ကြည့်ရပါမည် —
တကယ့် datacenter တွင် monitoring system နှင့် SSH ၂ မျိုးလုံး သုံးရသကဲ့သို့ ဖြစ်ပါသည်။
Dashboard က အမြင်မြန်၊ terminal က အသေးစိတ် — ၂ ခု ကိုက်ညီမှု ရှိ/မရှိ စစ်ပါ။

## 8. လုပ်ငန်းခွင် သတိပြုရန် အချက်များ

- Output ပါ ဂဏန်းများ (အပူချိန် ပုံမှန်အကွာအဝေး၊ power cap, driver/CUDA version) သည်
  GPU model, driver version, cooling design ပေါ် မူတည်၍ ပြောင်းလဲနိုင်ပါသည် —
  အပေါ်ပါ ဥပမာ output သည် illustrative သာ ဖြစ်ပါသည်။
- `P0` နှင့် `P2/P3` ကွာခြားချက်ကို ကောက်ချက် မချမီ power/temp column နှင့်
  clock အခြေအနေကို ပါ စစ်ဆေးပါ — performance state သည် workload အခြေအနေပေါ်လည်း
  မူတည်နိုင်ပါသည်။
- တစ်ခုတည်းသော column (ဥပမာ GPU-Util) ကို ကြည့်၍ fault အဆုံးအဖြတ် မပေးပါနှင့် —
  အခြား column များနှင့် dashboard နှင့် အမြဲ နှိုင်းယှဉ်ပါ။

## 9. Self-Check

1. `P0` နှင့် `P3` ကွာခြားချက်? Throttling ဘယ်လို သိသလဲ?
2. Util 0% + Memory ပြည့်နေလျှင် ဘာဖြစ်နိုင်သလဲ?
3. `topo -m` ထဲတွင် NV18 ဆိုတာ ဘာလဲ?
4. ECC error အသေးစိတ် ကြည့်လိုပါက ဘယ် command သုံးသနည်း?
5. Dashboard နှင့် `nvidia-smi` output မကိုက်ညီပါက ဘာလုပ်သနည်း?

**အဖြေ** — 1) P0 = full perf, P3 = reduced (throttle ဖြစ်နေ) — Perf column + power/temp ကြည့် · 2) hang သို့မဟုတ် memory leak · 3) NVLink 18 links ချိတ်ဆက်ထားခြင်း · 4) `nvidia-smi -q -d ECC` · 5) ၂ ခုလုံးကို ပြန်စစ်ပြီး မကိုက်ညီမှုကို မှတ်တမ်းတင် — monitoring data ဟောင်း/node မှား ကြည့်နေမိခြင်း ဖြစ်နိုင်

## 10. References

- nvidia-smi docs: https://docs.nvidia.com/deploy/nvidia-smi/
- NVIDIA System Management Interface: `man nvidia-smi`

## Technical review notes

- Output ဥပမာ (driver 535.129.03, CUDA 12.2) သည် illustrative ဖြစ်ပြီး version-dependent ဖြစ်သည် —
  တကယ့် environment တွင် ကွဲပြားနိုင်သည်။
- "Temp 35-75°C ပုံမှန်၊ 90°C+ thermal ပြဿနာ" ဆိုသော အကွာအဝေးသည် GPU model နှင့် cooling
  design ပေါ် မူတည်သည် — **needs verification** (platform spec sheet ဖြင့် တိုက်စစ်ပါ)။
- "P2/P3 = throttling" ဆိုခြင်းမှာ ရိုးရှင်းစွာ ရှင်းလင်းထားခြင်း ဖြစ်သည် — performance state
  သည် workload ပေါ်လည်း မူတည်နိုင်ပြီး throttling ကို clock/perf reason ဖြင့် အတည်ပြုသင့်သည်
  (**needs verification**)။
