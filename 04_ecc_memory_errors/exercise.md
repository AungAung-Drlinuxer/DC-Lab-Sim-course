# Module 04 — Exercise

## E1. ECC ဖတ်လေ့ကျင့်ခြင်း
1. `nvidia-smi -q -d ECC` run ပြီး GPU 0 ၏ volatile/aggregate counters များ မှတ်တမ်းတင်ပါ
2. Sandbox တွင် **ECC Error** inject ပြီး — counter ပြောင်းလဲမှု ပြန်ကြည့်ပါ
3. `nvidia-smi --reset-ecc-errors -i 0` run ပြီး — volatile counter ပြောင်း/aggregate မပြောင်း အတည်ပြုပါ

## E2. Severe ECC Scenario
1. **Complex Training Scenarios → Severe ECC Error** run ပါ
2. ဖြေရှင်းချက် ၃ ဆင့် (identify → decide → action) ရေးပါ —
   ဤအခြေအနေတွင် GPU ဆက်သုံး/မသုံး ဘာကြောင့် ဆုံးဖြတ်သနည်း
3. ဆုံးဖြတ်ချက်အတွက် လိုအပ်သော evidence (command output) ဘာတွေလဲ စာရင်းပြုစုပါ
