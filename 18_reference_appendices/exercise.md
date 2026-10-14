# Module 18 — လေ့ကျင့်ခန်း (Reference Lookup Drills)

> Appendix A/B/C ကို **ရှာတတ်အောင်** လေ့ကျင့်ခြင်း ဖြစ်ပါသည်။
> ပထမအကြိမ်တွင် table ဖွင့်ကြည့်ပြီး ဖြေပါ — အဖြေတွင် "ဘယ် appendix ရဲ့ ဘယ်နေရာက
> ရှာတွေ့ခဲ့သလဲ" ကိုပါ မှတ်ပါ။

## Drill 1 — Symptom → XID code + Severity + ပထမဆုံး အဆင့်

အောက်ပါ symptom တစ်ခုချင်းအတွက် (Appendix A ကို သုံး၍) ဖြည့်ပါ —
**XID code / Severity / ပထမဆုံး ဆောင်ရွက်သင့်သော အဆင့်** (A.4 triage rule ကို ကိုးကားပါ)။

1. `dmesg` ထဲတွင် "GPU has fallen off the bus" ပေါ်နေပြီး GPU ကို မတွေ့တော့ပါ။
2. NVLink link training မအောင်မြင်ကြောင်း error ပေါ်နေပြီး GPU-to-GPU ဆက်သွယ်မှု ပျက်နေပါသည်။
3. Training job တစ်ခု hang နေသော်လည်း `nvidia-smi` တွင် GPU က ရှိနေဆဲ — context သာ ပျက်သွားပုံ ရပါသည်။
4. Memory ထဲ double-bit ECC error ဖြစ်ပြီး job result များ ယုံ၍ မရတော့ပါ။
5. Driver version နှင့် GPU firmware version မကိုက်ညီကြောင်း error တက်နေပါသည်။
6. Row remapping ကို အောင်မြင်စွာ ပြုလုပ်ပြီးကြောင်း log တွင် တွေ့ရပါသည် — ဘာလုပ်ရမလဲ?

## Drill 2 — ဖော်ပြချက် → Glossary ဝေါဟာရ

အောက်ပါ ဖော်ပြချက်များနှင့် ကိုက်ညီသော ဝေါဟာရ (Appendix B) ကို ရှာပါ။
အကယ်၍ ဝေါဟာရ အတိုကောက် တစ်ခုတည်းက အဓိပ္ပာယ် ၂ မျိုး ရှိပါက ၂ ခုလုံး ဖော်ပြပါ။

1. InfiniBand switch ကွန်ရက်အတွင်း ကိုယ်တိုင် aggregation/reduction လုပ်ပေးသော
   in-network computing protocol။
2. GPU တစ်လုံးကို isolated GPU instance များအဖြစ် ခွဲဝေသုံးစွဲနိုင်စေသော feature။
3. Storage data ကို CPU system memory မဖြတ်ဘဲ GPU memory သို့ တိုက်ရိုက် ပို့ပေးသော technology။
4. IB subnet ကို စီမံခန့်ခွဲပြီး LID များ ခွဲဝေပေးသော component။ (⚠️ GPU ဘက်က အတိုကောက်
   တူနေတတ်သည် — ၂ မျိုးလုံး ရှင်းပြပါ)
5. NIC firmware image တစ်ခုတွင် ပါရှိပြီး board-specific firmware configuration ကို
   ဖော်ပြသော ID။
6. Server ကို OS ပေါ်မူတည်ဘဲ network မှ remote စီမံခန့်ခွဲနိုင်စေသော management controller။

## Drill 3 — လုပ်ငန်း → Command Family

အောက်ပါ လုပ်ငန်းတစ်ခုချင်းအတွက် မည်သည့် **command family** (Appendix C) ကို
သုံးရမည်နှင့် ဤ course တွင် သင်ထားသော ဥပမာ command တစ်ခု ဖော်ပြပါ။

1. Node တစ်ခုအတွင်း GPU တစ်လုံးချင်း၏ memory usage ကို real-time စောင့်ကြည့်ချင်ပါသည်။
2. InfiniBand port တစ်ခု၏ link state (Active/Down) နှင့် rate (HDR/NDR) ကို စစ်ချင်ပါသည်။
3. Server ရဲ့ ရုပ်ပိုင်း hardware inventory (CPU model, RAM) ကို firmware level မှ ကြည့်ချင်ပါသည်။
4. Cluster ပေါ်က job တစ်ခုချင်း၏ အခြေအနေနှင့် node allocation ကို scheduler မှတစ်ဆင့် ကြည့်ချင်ပါသည်။
5. 8-GPU all-reduce bandwidth ကို message size အမျိုးမျိုးဖြင့် တိုင်းတာချင်ပါသည်။
6. NIC firmware version ကို စစ်ဆေး update လုပ်ချင်ပါသည်။
