# Module 05 — Thermal & Power Management

## 1. ဘာကြောင့် အရေးကြီးသလဲ

DGX GPU တစ်လုံးသည် 400W (A100) မှ 1200W (B200) အထိ စားသုံးပြီး အပူထုတ်ပါသည် —
အပူစီမံခန့်ခွဲမှု ပျက်လျှင် **throttling → performance ကျ → hardware ပျက်စီး** အထိ ဖြစ်နိုင်ပါသည်။

## 2. ဖတ်နည်း ⭐

```bash
nvidia-smi -q -d TEMPERATURE    # GPU/memory အပူချိန်
nvidia-smi -q -d POWER          # power draw / cap / limit
ipmitool sensor list            # BMC sensors (temp, voltage, fan) ⭐
```

## 3. အပူချိန် အခြေအနေ ဇယား ⭐

| အပူချိန် | အခြေအနေ | လုပ်ရန် |
|---|---|---|
| 30-50°C | idle/ပေါ့ပါး — ပုံမှန် | — |
| 60-80°C | workload အောက် ပုံမှန် | စောင့်ကြည့် |
| 85-90°C+ | သတိပေး / throttle | airflow, fan, rack စစ် |
| 90-100°C (အားလုံး) | **စနစ်တကျ ပြဿနာ** (complex scenario) | cooling စနစ် စစ် |

⚠️ GPU တစ်လုံးသာ ပူနေလျှင် GPU ပြဿနာ၊ **GPU အားလုံး** ပူနေလျှင် cooling/airflow/room ပြဿနာ —
ဤခွဲခြားချက်က စာမေးပွဲတွင် အမေးများပါသည်။ ⭐

## 4. Throttling (Perf State ကျခြင်း) ⭐

`nvidia-smi` ထဲက **Perf** column —
- `P0` = အပြည့် performance
- `P2/P3` = clock လျှော့ထားခြင်း (thermal သို့ power limit ကြောင့်)

throttling ဖြစ်နေလျှင် စစ်ရန် —
1. အပူ များနေလား (`-d TEMPERATURE`)
2. power cap ကျော်နေလား (`-d POWER`)
3. workload က တကယ် ကြီးနေလား (util)

## 5. Power စီမံခန့်ခွဲမှု ⭐

```bash
nvidia-smi -pl 400            # GPU 0 ၏ power limit ကို 400W သတ်မှတ်
nvidia-smi -q -d POWER        # limit/cap အတည်ပြု
```
- **Usage/Cap** — လက်ရှိ သုံးနှုန်း / အမြင့်ဆုံး
- power limit လျှော့ချခြင်း = စွမ်းဆောင်ရည် လျှော့ခြင်း — rack power budget ကန့်သတ်ချက်ကြောင့် လုပ်ရလေ့ရှိ

⚠️ simulator ထဲ remediation တွင် `nvidia-smi -i 0 -pl 500` ကဲ့သို့ command ပါ — power cap ကျော်ပြဿနာ ဖြေရှင်းရာတွင် ပြန်လည် သတ်မှတ်ခြင်း။

## 6. Datacenter Cooling အခြေခံ ⭐

```
Cold aisle (အေး)  →  server intake  →  GPU  →  Hot aisle (ပူ)  →  CRAC/chiller
```
- **Air cooling** — rack 40-50kW အထိ အဆင်ပြေ
- **Liquid cooling** — 40-50kW ကျော် rack များအတွက် လိုအပ် (GB200 NVL72 ~120kW ဆို liquid မဖြစ်မနေ)
- fan ပျက် / airflow ပိတ် / hot aisle recirculation = အပူတက် အကြောင်းရင်း များ

`ipmitool sensor list` ထဲက fan speed, inlet temp များက cooling ကျန်းမာရေး ညွှန်းကိန်း ဖြစ်ပါသည်။

## 7. Self-Check

1. GPU အားလုံး 90°C+ ဖြစ်နေလျှင် ပထမ စဉ်းစားရမည့် အကြောင်းရင်း?
2. Perf P3 ဆိုတာ ဘာကို ဆိုလိုသလဲ။
3. air cooling limit နှင့် liquid cooling လိုအပ်မှု?

**အဖြေ** — 1) cooling စနစ်/airflow/room (GPU တစ်လုံးတည်း မဟုတ်) · 2) clock throttle ဖြစ်နေခြင်း · 3) ~40-50kW/rack ကျော်လျှင် liquid

## 8. References

- GPU temperature/power: `nvidia-smi` docs
- Datacenter cooling: NVIDIA Validated Designs (DGX)
- ipmitool: IPMItool user guide
