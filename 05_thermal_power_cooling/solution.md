# Module 05 — Solution

## E1
- inject ပြီး: GPU temp 90°C+ တက်၊ `ipmitool sensor list` တွင် သက်ဆိုင်ရာ sensor ပြောင်း
- ပြန်ပြင်ပြီး: temp ပုံမှန် ပြန်ဆင်း

## E2
- Usage/Cap ကျော်နေမှု တွေ့ရ
- `-pl 500` = power limit ပြန်တိုး/သတ်မှတ် — cap နှင့် ကိုက်ညီပါက ပြဿနာ ပြေလည်

## E3
- အားလုံး ပူနေလျှင်: room cooling, CRAC, airflow, hot aisle recirculation, rack အတွင်း ပိတ်ဆို့မှု
- GPU တစ်လုံးတည်း ဆိုလျှင်: GPU ကိုယ်တိုင် fan/heatsink/thermal paste
- plan: ① identify scope (1 vs all) ② ipmitool/NVSM ဖြင့် system cooling စစ် ③ workload ရပ်ပြီး ဖြေရှင်း/cooling ပြင် → validate
