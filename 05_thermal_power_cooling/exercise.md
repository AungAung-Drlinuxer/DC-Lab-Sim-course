# Module 05 — Exercise

## E1. Thermal fault
1. Sandbox တွင် GPU 3 ၌ **Thermal Issue** inject ပါ
2. `nvidia-smi -q -d TEMPERATURE` — GPU/memory temp မှတ်ပါ
3. `ipmitool sensor list` — inlet/fan sensors ပြောင်းလဲမှု ကြည့်ပါ
4. **Clear All** ဖြင့် ပြန်ပြင်ပြီး ပုံမှန် ဖြစ်သွားကြောင်း အတည်ပြုပါ

## E2. Power fault
1. **Power Issue** (power limit exceeded) inject ပါ
2. `nvidia-smi -q -d POWER` — Usage vs Cap ကြည့်ပါ
3. `nvidia-smi -i 0 -pl 500` remediation ကို စမ်းသပ်ပြီး ရလဒ် မှတ်ပါ

## E3. Thermal Alert scenario
1. **Complex Scenarios → Thermal Alert (All GPUs 90-100°C)** run ပါ
2. GPU တစ်လုံးတည်း ပူခြင်းနှင့် အားလုံး ပူခြင်း မတူကြောင်း — အကြောင်းရင်း ခွဲခြား ရှင်းပြပါ
3. ဖြေရှင်းရန် plan ၃ ဆင့် ရေးပါ
