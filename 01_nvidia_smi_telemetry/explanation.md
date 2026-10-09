# Module 01 — nvidia-smi & GPU Telemetry

## 1. nvidia-smi က ဘာလဲ

`nvidia-smi` (NVIDIA System Management Interface) သည် GPU ၏ **state တစ်ခုလုံး** ကို
တစ်နေရာတည်းတွင် ပြသော tool ဖြစ်ပါသည် — device state, memory, temperature, power,
ECC, processes, topology အားလုံး။ စာမေးပွဲတွင် output ဖတ်ရှုရေး ကို အများဆုံး မေးပါသည်။

## 2. Default Output ဖတ်နည်း ⭐

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

## 3. Utilization ဖတ်ခြင်း — ၃ ပုံစံ ⭐

| အခြေအနေ | အဓိပ္ပာယ် |
|---|---|
| Util 95-100% + Mem မြင့် | training workload ပုံမှန် ဖြစ်နေ |
| Util 0% + Mem 50-200MB | idle (workload မရှိ) — ပုံမှန် |
| Util 0% + Mem ပြည့်နေ | hang / memory leak — ပြဿနာ |
| Util မမြင့် + power cap ကျော် | power ပြဿနာ |

⚠️ "GPU util နည်းနေ" ဆိုတာ အမြဲ fault မဟုတ်ပါ — workload ရပ်နေခြင်းလည်း ဖြစ်နိုင်ပါသည်။

## 4. Query Options အရေးကြီးသောများ ⭐

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

## 5. Topology (`topo -m`) ဖတ်နည်း

```
        GPU0  GPU1  GPU2  GPU3
GPU0     X    NV18  NV18  NV18
GPU1    NV18   X    NV18  NV18
```
- **NV18/NV12** = NVLink (ဂဏန်း = link အရေအတွက်) — bandwidth အမြင့်ဆုံး
- **PIX/PIX** = same PCIe switch — သာမန်
- **SYS** = system (NUMA ကျော်) — အနှေးဆုံး
- training အတွက် NVLink အုပ်စု အတွင်း ထားခြင်းက အကောင်းဆုံး

## 6. Dashboard နှင့် နှိုင်းယှဉ် ဖတ်ခြင်း

DC Lab Sim တွင် dashboard (visual) နှင့် terminal (`nvidia-smi`) ၂ ခုလုံး ကြည့်ရပါမည် —
တကယ့် datacenter တွင် monitoring system နှင့် SSH ၂ မျိုးလုံး သုံးရသကဲ့သို့ ဖြစ်ပါသည်။
Dashboard က အမြင်မြန်၊ terminal က အသေးစိတ် — ၂ ခု ကိုက်ညီမှု ရှိ/မရှိ စစ်ပါ။

## 7. Self-Check

1. `P0` နှင့် `P3` ကွာခြားချက်? Throttling ဘယ်လို သိသလဲ?
2. Util 0% + Memory ပြည့်နေလျှင် ဘာဖြစ်နိုင်သလဲ?
3. `topo -m` ထဲတွင် NV18 ဆိုတာ ဘာလဲ?

**အဖြေ** — 1) P0 = full perf, P3 = reduced (throttle ဖြစ်နေ) — Perf column + power/temp ကြည့် · 2) hang သို့မဟုတ် memory leak · 3) NVLink 18 links ချိတ်ဆက်ထားခြင်း

## 8. References

- nvidia-smi docs: https://docs.nvidia.com/deploy/nvidia-smi/
- NVIDIA System Management Interface: `man nvidia-smi`
