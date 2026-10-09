# Module 04 — ECC & Memory Errors

## 1. ECC က ဘာလဲ

ECC (Error-Correcting Code) သည် GPU memory (HBM) ထဲက data corruption ကို
ဖော်ထုတ်ပြီး ပြင်ပေးသော mechanism ဖြစ်ပါသည် — datacenter GPU များတွင်
**enabled ဖြစ်ရ**ပါမည် (consumer GPU များတွင် ECC မပါ)။

## 2. Error အမျိုးအစား ၂ မျိုး ⭐

| အမျိုးအစား | အဓိပ္ပာယ် | ဆက်လုပ်ရန် |
|---|---|---|
| **Correctable** | ECC က ကိုယ်တိုင် ပြင်ပေးပြီး | စောင့်ကြည့် — များလာလျှင် ပြဿနာ |
| **Uncorrectable** | ပြင်၍ မရ — data ပျက် | ပြင်းထန် — GPU replace/RMA စဉ်းစား |

## 3. ဖတ်နည်း ⭐

```bash
nvidia-smi -q -d ECC
```
ထွက်လာသော field များ —
- **Volatile** — နောက်ဆုံး reset မှ စ၍ ရရှိသော error (လက်ရှိ အခြေအနေ)
- **Aggregate** — GPU တစ်သက်တာ စုစုပေါင်း (သမိုင်း)
- **Uncorrectable volatile/aggregate** — မသုည ဆိုလျှင် ပြင်းထန်ပြဿနာ

```bash
nvidia-smi --reset-ecc-errors -i 0   # ECC counters reset (volatile သာ)
```
⚠️ Aggregate ကို reset လုပ်၍ မရပါ — history အမြဲ ကျန်ပါမည်။

## 4. Page Retirement / Row Remapping ⭐

ပျက်သွားသော memory block များကို GPU firmware က **retire/retire လုပ်ပြီး**
အသုံးပြုခြင်းမှ ဖယ်ရှားပါသည် —
- အနည်းငယ် ဆိုလျှင် GPU ဆက်အသုံးပြုနိုင် (available memory လျော့သွား)
- **retired pages အများကြီး** ဆိုလျှင် GPU အိုမင်း/ပျက်စီးနေ — replace စဉ်းစား
- ဤလုပ်ဆောင်ချက်ကို "row remapping" (Ampere+) ဟုလည်း ခေါ်ပါသည်

## 5. ECC ပြဿနာ ဆုံးဖြတ်ချက် tree ⭐

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

## 6. စာမေးပွဲ trap

- ECC **enabled ရှိခြင်း** နှင့် error **မရှိခြင်း** မတူပါ — enabled ဖြစ်မှ error ဖမ်းနိုင်
- volatile (reset ရ) vs aggregate (reset မရ) ကွာခြားချက်
- uncorrectable single event = စောင့်ကြည့်၊ repeated = replace

## 7. Self-Check

1. Correctable နှင့် uncorrectable ECC error ကွာခြားချက်?
2. Aggregate ECC counter ကို reset လို့ ရ/မရ?
3. Page retirement က ဘာလဲ။

**အဖြေ** — 1) ပြင်၍ရ/မရ — ဆက်လက် ဆောင်ရွက်ပုံ ကွာ · 2) မရ (history ကျန်) · 3) ပျက်သော memory block ကို firmware ဖြင့် ဖယ်ရှား အသုံးပြုခြင်းမှ ကာကွယ်ခြင်း

## 8. References

- ECC Management: https://docs.nvidia.com/deploy/ecc/
- XID 63/94/95 (ECC-related): https://docs.nvidia.com/deploy/xid-errors/
