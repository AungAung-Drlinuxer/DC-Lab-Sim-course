# Module 02 — Exercise

## E1. Discovery & dmon
1. Terminal တွင် `dcgmi discovery -l` run ပြီး GPU အရေအတွက် ရေတွက်ပါ
2. `dcgmi dmon -e 1001,1002,1004` run ပြီး 10 စက္ကန့် ကြည့်ပါ — ကိန်းဂဏန်း ၃ ခု ဘာကို ကိုယ်စားပြုသနည်း
3. Workload (Training 95%) apply ပြီး dmon ပြန်ကြည့် — ဘာပြောင်းသွားသနည်း

## E2. diag အဆင့်များ
1. `dcgmi diag -r 1` run ပါ — ရလဒ် မှတ်တမ်းတင်ပါ
2. Sandbox တွင် **ECC Error** inject ပြီး `dcgmi diag -r 1` ပြန် run ပါ — ပြောင်းလဲမှု တွေ့/မတွေ့
3. `dcgmi health -s` run ပြီး health watch အခြေအနေ ကြည့်ပါ

## E3. NVSM
1. `nvsm show health` run ပြီး node ၏ health summary ကို အနှစ်ချုပ် ရေးပါ
2. Fault တစ်ခု (XID Error) inject ပြီး `nvsm show health` ပြန်ကြည့်ပါ
