# Module 05 — Thermal & Power Management

## 1. ပြဿနာ — ဘာကြောင့် အရေးကြီးသလဲ

DGX GPU တစ်လုံးသည် A100 SXM (400W) မှ B200 (standard 1,000W — configuration ပေါ်မူတည်၍
1,200W အထိ) အထိ စားသုံးပြီး အပူ အများအပြား ထုတ်ပါသည် — GPU တစ်လုံး၏ TDP သည်
မော်ဒယ်နှင့် configuration ပေါ် မူတည်၍ ကွာခြားပါသည်။ အပူစီမံခန့်ခွဲမှု ပျက်လျှင်
**throttling → performance ကျ → hardware ပျက်စီး** အထိ ဆက်လက် ဖြစ်ပွားနိုင်ပါသည်။
လုပ်ငန်းသုံးအဆင့်တွင် အပူ/power ပြဿနာကို အချိန်မီ ဖော်ထုတ်နိုခြင်းသည် GPU ရင်းနှီးမြှုပ်နှံမှုကို
ကာကွယ်ရာ ရောက်ပါသည်။

## 2. Core Concept — Throttling နှင့် Power Cap

GPU သည် အပူ သို့မဟုတ် power သတ်မှတ်ချက် ကျော်လွန်လျှင် ကိုယ့်ကိုယ်ကို ကာကွယ်ရန်
clock speed ကို လျှော့ချပါသည် — ဤအခြေအနေကို **throttling** ဟု ခေါ်ပြီး
`nvidia-smi` ထဲက **Perf** column တွင် မြင်ရပါသည် —

- `P0` = အပြည့် performance
- `P2/P3` = clock လျှော့ထားခြင်း (thermal သို့ power limit ကြောင့်)

**Power cap** ဆိုသည်မှာ GPU တစ်လုံး စားသုံးရန် ခွင့်ပြုထားသော အမြင့်ဆုံး power ဖြစ်ပြီး
လက်ရှိ စားသုံးနှုန်း (usage) နှင့် နှိုင်းယှဉ်၍ ကြည့်ရပါမည်။ power limit ကို လျှော့ချခြင်းသည်
စွမ်းဆောင်ရည် လျှော့ချခြင်းပင် ဖြစ်ပါသည် — rack power budget ကန့်သတ်ချက်ကြောင့်
ရည်ရွယ်ချက်ရှိရှိ လုပ်ရလေ့ ရှိပါသည်။

## 3. အပူချိန် အခြေအနေ နှိုင်းယှဉ် ဇယား ⭐

| အပူချိန် | အခြေအနေ | လုပ်ရန် |
|---|---|---|
| 30-50°C | idle/ပေါ့ပါး — ပုံမှန် | — |
| 60-80°C | workload အောက် ပုံမှန် | စောင့်ကြည့် |
| 85-90°C+ | သတိပေး / throttle | airflow, fan, rack စစ် |
| 90-100°C (အားလုံး) | **စနစ်တကျ ပြဿနာ** (complex scenario) | cooling စနစ် စစ် |

⚠️ ဤဇယားသည် ခန့်မှန်း လမ်းညွှန်ချက်သာ ဖြစ်ပါသည် — အမှန်တကယ် threshold (slowdown/shutdown
temperature) သည် GPU မော်ဒယ်ပေါ် မူတည်၍ ကွာခြားပါသည်။ တိကျသော အတိုင်းအတာကို
`nvidia-smi -q -d TEMPERATURE` ထဲက threshold field များဖြင့် အတည်ပြုပါ။

အပူတက်သောအခါ **ဘယ်နှစ်လုံး ပူနေသလဲ** ဆိုသည်က အရေးကြီးသော ခွဲခြားချက် ဖြစ်ပါသည် — ⭐

| ပူနေသော GPU | ယေဘုယျ အကြောင်းရင်း | စစ်ဆေးရန် |
|---|---|---|
| တစ်လုံးသာ | GPU ကိုယ်တိုင် ပြဿနာ (ယိုယွင်းနေခြင်း၊ paste/fan) | ထို GPU ကို အာရုံစိုက် |
| အားလုံး | cooling/airflow/room ပြဿနာ | cooling စနစ် အာရုံစိုက် |

ဤခွဲခြားချက်က စာမေးပွဲတွင် အမေးများပါသည် — GPU တစ်လုံး ပူခြင်းကို cooling ပြဿနာဟု
မှားယွင်း ဖြေဆိုခြင်းကို ရှောင်ပါ။

### Air cooling နှင့် Liquid cooling နှိုင်းယှဉ်ခြင်း ⭐

| | Air cooling | Liquid cooling |
|---|---|---|
| Rack density | ယေဘုယျအားဖြင့် rack 40-50kW အထိ အဆင်ပြေ | 40-50kW ကျော် rack များအတွက် လိုအပ် |
| ဥပမာ | DGX A100/H100 အများစု | GB200 NVL72 (~120kW — liquid မဖြစ်မနေ) |
| အားသာချက်/အားနည်းချက် | ရိုးရှင်း၊ ကုန်ကျစရိတ် နည်း၊ density ကန့်သတ် | density မြင့်၊ စနစ် ပိုရှုပ်ထွေး |

rack power density အတွက် 40-50kW ကဲ့သို့ ဂဏန်းများသည် facility design ပေါ် မူတည်သော
ခန့်မှန်းချက်များ ဖြစ်ပါသည် (needs verification — site-specific)။

## 4. Practical — ဖတ်နည်းနှင့် power သတ်မှတ်ခြင်း ⭐

```bash
nvidia-smi -q -d TEMPERATURE    # GPU/memory အပူချိန်နှင့် threshold များ ကြည့်
nvidia-smi -q -d POWER          # power draw / cap / limit ကြည့်
ipmitool sensor list            # BMC sensors — fan speed, inlet temp, voltage ⭐
```

`ipmitool sensor list` ထဲက fan speed, inlet temp များသည် chassis/room level cooling
ကျန်းမာရေး ညွှန်းကိန်း ဖြစ်ပြီး GPU တစ်လုံးချင်းစီထက် ပိုကျယ်သော ရှုထောင့် ပေးပါသည်။

```bash
nvidia-smi -i 0 -pl 400         # GPU 0 ၏ power limit ကို 400W သတ်မှတ်
nvidia-smi -q -d POWER          # limit/cap အတည်ပြု
```

`-pl` (power limit) ဖြင့် power cap ကို ပြန်လည် သတ်မှတ်ပါသည် —
- **Usage/Cap** — လက်ရှိ စားသုံးနှုန်း / အမြင့်ဆုံး သတ်မှတ်ချက်
- သတိပြုရန် — သတ်မှတ်နိုင်သော power limit အပိုင်းအခြားသည် GPU မော်ဒယ်ပေါ် မူတည်ပါသည်
  (ဥပမာ command များသည် illustrative ဖြစ်ပြီး ကိုယ့် GPU ၏ ခွင့်ပြုချက်အတိုင်း သတ်မှတ်ရပါမည်)

⚠️ simulator ထဲ remediation တွင် `nvidia-smi -i 0 -pl 500` ကဲ့သို့ command ပါဝင်ပါသည် —
power cap ကျော်ပြဿနာ ဖြေရှင်းရာတွင် power limit ကို ပြန်လည် သတ်မှတ်ခြင်း ဖြစ်ပါသည်။

### Datacenter Cooling အခြေခံ ⭐

```
Cold aisle (အေး)  →  server intake  →  GPU  →  Hot aisle (ပူ)  →  CRAC/chiller
```

- အေးသော လေကို server ရှေ့မှ သွင်း၊ GPU ကျော်ပြီး ပူသွားသော လေကို hot aisle မှ ပြန်ထုတ်၍
  CRAC/chiller ဖြင့် အေးအောင် ပြန်လုပ်ပါသည်။
- fan ပျက်ခြင်း၊ airflow ပိတ်ခြင်း၊ hot aisle မှ ပူသောလေ ပြန်ဝင်ခြင်း (recirculation) တို့သည်
  အပူတက်ခြင်း၏ အဖြစ်များသော အကြောင်းရင်းများ ဖြစ်ပါသည်။

## 5. လုပ်ငန်းသုံး သတိပေးချက်များ (Operational Cautions)

- throttling ဖြစ်နေလျှင် ဦးစွာ ခွဲခြားပါ — ① အပူ များနေလား (`-d TEMPERATURE`)
  ② power cap ကျော်နေလား (`-d POWER`) ③ workload က တကယ် ကြီးနေလား (util)။
- power limit လျှော့ခြင်း/တင်ခြင်းသည် workload စွမ်းဆောင်ရည်ကို တိုက်ရိုက် သက်ရောက်ပါသည် —
  change log မှတ်ပြီး ပြောင်းလဲပါ။
- `ipmitool` ဖြင့် BMC sensor ဖတ်ခြင်းသည် OS ပေါ်က GPU tool များနှင့် သီးခြား
  ရှုထောင့် ပေးပါသည် — GPU tool များ ပြဿနာ မပြလျှင်လည်း chassis-level ကို စစ်ပါ။
- အပူချိန် အပိုင်းအခြားများနှင့် rack density ဂဏန်းများသည် ခန့်မှန်းချက်များ ဖြစ်ပြီး
  မော်ဒယ်/site ပေါ် မူတည်ပါသည် — တိကျသော threshold ကို မိမိ hardware ၏ documentation ဖြင့်
  အတည်ပြုပါ။

## 6. Self-Check

1. GPU အားလုံး 90°C+ ဖြစ်နေလျှင် ပထမ စဉ်းစားရမည့် အကြောင်းရင်း ဘာလဲ။
   **အဖြေ** — cooling စနစ်/airflow/room ပြဿနာ — GPU တစ်လုံးတည်း၏ ပြဿနာ မဟုတ်ပါ (GPU တစ်လုံးသာ ဆိုလျှင် ထို GPU ကို စစ်ပါ)။
2. Perf P3 ဆိုတာ ဘာကို ဆိုလိုသလဲ။
   **အဖြေ** — clock လျှော့ထားခြင်း (thermal သို့ power limit ကြောင့် throttling) ဖြစ်နေခြင်း ဖြစ်ပါသည်။
3. air cooling limit နှင့် liquid cooling လိုအပ်မှု ဘယ်လို ခွဲခြားသလဲ။
   **အဖြေ** — rack power density ခန့်မှန်း 40-50kW ကျော်လျှင် liquid cooling လိုအပ်ပြီး GB200 NVL72 ကဲ့သို့ rack များတွင် liquid မဖြစ်မနေ ဖြစ်ပါသည်။
4. throttling ဖြစ်နေလျှင် မည်သည့် အချက် ၃ ချက် စစ်ရမလဲ။
   **အဖြေ** — ① အပူချိန် ② power cap/limit ③ workload utilization — တို့ကို အစဉ်လိုက် စစ်ပါ။

## 7. References

- GPU temperature/power: `nvidia-smi` docs
- Datacenter cooling: NVIDIA Validated Designs (DGX)
- ipmitool: IPMItool user guide

## Technical review notes

- B200 TDP ကို "1,200W" ဟု တစ်သမတ်တည်း မပြဘဲ — standard configuration 1,000W၊
  configuration ပေါ်မူတည်၍ 1,200W အထိ ဟု ပြင်ဆင်ထားပါသည် (NVIDIA HGX B200
  "configurable up to 1 kW per GPU" မှ အတည်ပြု)။
- အပူချိန် အပိုင်းအခြား ဇယားနှင့် air/liquid cooling အတွက် 40-50kW ကဲ့သို့ ဂဏန်းများသည်
  ခန့်မှန်းချက်များ ဖြစ်ပြီး GPU မော်ဒယ်/site design ပေါ် မူတည်သဖြင့် **needs verification**
  အဖြစ် မှတ်သားထားပါသည်။
- GB200 NVL72 rack power (~120kW) သည် အများအားဖြင့် ဖော်ပြလေ့ရှိသော ဂဏန်းဖြစ်သော်လည်း
  configuration ပေါ် မူတည်နိုင်၍ **needs verification** ဖြစ်ပါသည်။
