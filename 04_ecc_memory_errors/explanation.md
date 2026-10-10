# Module 04 — ECC & Memory Errors

## 1. ပြဿနာ — ဘာကြောင့် အရေးကြီးသလဲ

ရက်သတ္တပတ်များစွာ ဆက်တိုက် run သော training job တစ်ခုတွင် GPU memory (HBM) ထဲက bit တစ်ခု ပျက်သွားလျှင်
model weight ပျက်ပြီး loss မထင်မှတ်ဘဲ တက်ခြင်း၊ ရလဒ် တိတ်တဆိတ် ပျက်ခြင်း (silent data corruption) အထိ
ဖြစ်နိုင်ပါသည်။ ထို့ကြောင့် datacenter GPU များတွင် memory error ကို ဖော်ထုတ်ပြီး ပြင်ပေးသော **ECC**
ကို ထည့်သွင်းပေးထားပါသည် — datacenter GPU များတွင် ECC **enabled ဖြစ်ရ**ပါမည်။

ဤ chapter တွင် ECC error ၂ မျိုး ခွဲခြားနည်း၊ counter ဖတ်နည်း၊ ပျက်စီးသွားသော memory block များကို
မည်သို့ ဖယ်ရှားသလဲ၊ မည်သည့်အခါ GPU replace/RMA လုပ်ရမည် ဆုံးဖြတ်မည်တို့ကို လေ့လာပါမည်။

## 2. Core Concept — ECC က ဘယ်လို အလုပ်လုပ်သလဲ

ECC (Error-Correcting Code) သည် memory ထဲ ရေးသားသော data နှင့်အတူ code/parity bits များ
ထည့်သိမ်းပေးခြင်းဖြင့် —

- တစ် bit ပျက်ခြင်း (single-bit error) ကို ဖော်ထုတ်ပြီး **ကိုယ်တိုင် ပြင်ပေး**နိုင်ပါသည် —
  ထိုအခါကို correctable error ဟု ခေါ်ပါသည်။
- တစ် bit ထက် များသော ပျက်စီးမှု (multi-bit error) ကို ဖော်ထုတ်နိုင်သော်လည်း ပြင်၍ **မရ**နိုင်ပါ —
  ထိုအခါကို uncorrectable error ဟု ခေါ်ပြီး data ပျက်သွားပါပြီ။

consumer GPU (GeForce) များတွင် ECC မပါဝင်ပါ။ စာမေးပွဲတွင် "ECC က ဘာကို ကာကွယ်ပေးသလဲ" ဆိုလျှင်
GPU memory (HBM) ထဲက data corruption ကို ဖြစ်ပြီး၊ host RAM သို့မဟုတ် storage ကို မဟုတ်ပါ။

## 3. Error အမျိုးအစား ၂ မျိုး နှိုင်းယှဉ်ခြင်း ⭐

| အမျိုးအစား | အဓိပ္ပာယ် | ဆက်လုပ်ရန် |
|---|---|---|
| **Correctable** | ECC က ကိုယ်တိုင် ပြင်ပေးပြီး | စောင့်ကြည့် — အကြိမ်ရေ များလာလျှင် ပြဿနာ |
| **Uncorrectable** | ပြင်၍ မရ — data ပျက်သွားပြီး | ပြင်းထန် — GPU replace/RMA စဉ်းစား |

counter ၂ မျိုးလည်း ရှိပါသည် —

| Counter | အဓိပ္ပာယ် | Reset ရ/မရ |
|---|---|---|
| **Volatile** | နောက်ဆုံး reset (driver load) မှ စ၍ ရရှိသော error — လက်ရှိ အခြေအနေ | ရ |
| **Aggregate** | GPU သက်တမ်း တစ်လျှောက်လုံး စုစုပေါင်း — သမိုင်းမှတ်တမ်း | မရ (history အမြဲ ကျန်) |

## 4. Practical — ဖတ်နည်းနှင့် reset လုပ်နည်း ⭐

```bash
nvidia-smi -q -d ECC
```
GPU အလိုက် ECC counters အားလုံး (volatile/aggregate, correctable/uncorrectable) ကို ပြပေးပါသည်။
ထွက်လာသော field များထဲ —

- **Volatile** — နောက်ဆုံး reset မှ စ၍ ရရှိသော error (လက်ရှိ အခြေအနေ)
- **Aggregate** — GPU တစ်သက်တာ စုစုပေါင်း (သမိုင်း)
- **Uncorrectable volatile/aggregate** — မသုည ဆိုလျှင် ပြင်းထန်ပြဿနာ — အနီရောင် အလံ ဖြစ်ပါသည်

```bash
nvidia-smi --reset-ecc-errors -i 0
```
GPU 0 ၏ **volatile** ECC counters ကိုသာ reset လုပ်ပါသည် — စောင့်ကြည့်မှု အသစ် စချင်လျှင် သုံးပါသည်။

⚠️ **Aggregate ကို reset လုပ်၍ မရပါ** — history အမြဲ ကျန်ပါမည်။ RMA ဆုံးဖြတ်ရာတွင် aggregate
ကိုပင် ပြန်ကြည့်ရသဖြင့် reset ခြင်းက ပြဿနာကို ဖျောက်ပေးခြင်း မဟုတ်ပါ။

## 5. Page Retirement / Row Remapping ⭐

ပျက်စီးသွားသော memory block များကို GPU firmware/driver က မှတ်တမ်းတင်ပြီး အသုံးပြုခြင်းမှ
ဖယ်ရှားပေးပါသည် —

- Ampere မျိုးဆက် (A100) နောက်ပိုင်းတွင် **row remapping**၊ ယခင် မျိုးဆက်များတွင်
  **dynamic page retirement** ဟု ခေါ်ပါသည်။
- ပျက်စီးမှု အနည်းငယ် ဆိုလျှင် GPU ဆက်အသုံးပြုနိုင်ပါသည် — available memory အနည်းငယ် လျော့သွားပါမည်။
- **retired pages/remapped rows အများကြီး** ဆိုလျှင် GPU ပျက်စီးနေခြင်း လက္ခဏာ — replace စဉ်းစားပါ။
- remap entry သည် မှတ်တမ်းတင်ပြီးမှ **pending** အဖြစ် ကျန်နိုင်ပြီး GPU reset/reboot ပြုလုပ်မှ
  အမှန်တကယ် apply ဖြစ်ပါသည် — ထိုအခြေအနေကို XID 63 က ဖော်ပြလေ့ ရှိပါသည်။

## 6. ဆုံးဖြတ်ချက် (Decision) Tree ⭐

```
ECC error တွေ့
   ↓
correctable လား?
   ├─ ရှားရှားပါးပါး → စောင့်ကြည့် (log ဆက်မှတ်)
   ├─ အမြဲတမ်း တက်နေ → GPU replace စဉ်းစား
   ↓
uncorrectable လား?
   ├─ တစ်ကြိမ်တည်း → reset ECC + monitor + burn-in စစ်
   └─ ထပ်တက် → GPU replace (RMA) ⭐
```

ECC error များသည် system log ထဲတွင် XID error အဖြစ် တက်လေ့ ရှိပါသည် —
XID 92 (single-bit error နှုန်း မြင့်ခြင်း)၊ XID 48 (uncorrectable error)၊ XID 94/95
(contained/uncontained error — A100 မှစ၍ error containment ရှိသော GPU များ)၊ XID 63/64
(page retirement/row remapping မှတ်တမ်း အောင်မြင်/မအောင်မြင်) တို့ ဖြစ်ပါသည်။
XID 94 = application တစ်ခုတည်းသို့ ကန့်သတ်နိုင် (app restart)၊ XID 95 = ကန့်သတ်၍ မရ
(GPU reset လိုအပ်) — ခွဲခြားမှတ်သားပါ။

## 7. လုပ်ငန်းသုံး သတိပေးချက်များ (Operational Cautions)

- ECC **enabled ရှိခြင်း** နှင့် error **မရှိခြင်း** မတူပါ — enabled ဖြစ်မှ error ကို ဖမ်းမိနိုင်ခြင်း ဖြစ်ပါသည်။
- volatile (reset ရ) နှင့် aggregate (reset မရ) ကွာခြားချက်ကို မှတ်ထားပါ —
  reset လုပ်လိုက်ခြင်းက error ဖြစ်ခဲ့ခြင်းကို ဖျက်ပစ်ခြင်း မဟုတ်ပါ။
- uncorrectable single event = ဆက်လက် စောင့်ကြည့်၊ repeated = replace/RMA။
- ဤနေရာက command များနှင့် field အမည်များသည် driver ဗားရှင်းပေါ် မူတည်၍ output အသေးစိတ်
  ကွာနိုင်ပါသည် — လက်တွေ့တွင် မိမိ driver ဗားရှင်း၏ output ဖြင့် အတည်ပြုပါ။

## 8. Self-Check

1. Correctable နှင့် uncorrectable ECC error ကွာခြားချက် ဘာလဲ။
   **အဖြေ** — Correctable က ECC က ကိုယ်တိုင် ပြင်ပေးပြီး၊ uncorrectable က ပြင်၍ မရဘဲ data ပျက်သွားခြင်း — ဆက်လက် ဆောင်ရွက်ပုံ ကွာခြားပါသည်။
2. Aggregate ECC counter ကို reset လို့ ရ/မရ။
   **အဖြေ** — မရပါ။ volatile counter ကိုသာ `--reset-ecc-errors` ဖြင့် reset လုပ်၍ ရပြီး၊ history အမြဲ ကျန်ပါမည်။
3. Page retirement / row remapping ဆိုတာ ဘာလဲ။
   **အဖြေ** — ပျက်စီးသော memory block များကို firmware/driver ဖြင့် မှတ်တမ်းတင် ဖယ်ရှားပြီး အသုံးပြုခြင်းမှ ကာကွယ်ခြင်း ဖြစ်ပါသည် — အများကြီး ပျက်လျှင် GPU replace စဉ်းစားရပါမည်။
4. Uncorrectable error တစ်ကြိမ် တက်လာလျှင် ဘာလုပ်ရမလဲ။
   **အဖြေ** — ECC counter reset + ဆက်လက်စောင့်ကြည့် + burn-in စစ်ဆေးပြီး၊ ထပ်တက်လာလျှင် GPU replace (RMA) ဆောင်ရွက်ပါ။

## 9. References

- ECC Management: https://docs.nvidia.com/deploy/ecc/
- XID 63/94/95 (ECC-related): https://docs.nvidia.com/deploy/xid-errors/

## Technical review notes

- XID 92/94/95 နှင့် XID 63/64 ၏ အဓိပ္ပာယ်များ (high single-bit error rate / contained / uncontained /
  row-remapping recording success/failure) ကို NVIDIA Xid catalog (docs.nvidia.com/deploy/xid-errors)
  ဖြင့် အတည်ပြုပြီး — မူရင်း reference line ကို အနည်းငယ် ဖြည့်စွက်ထားပါသည်။
- "row remapping (Ampere+)" နှင့် "dynamic page retirement (ယခင်မျိုးဆက်)" ခွဲခြားချက်ကို
  NVIDIA GPU Memory Error Management စာရွက်စာတမ်းဖြင့် အတည်ပြုပြီး။
- ပြင်ဆင်ရန် ကျန်ရှိသော အချက် မတွေ့ပါ။
