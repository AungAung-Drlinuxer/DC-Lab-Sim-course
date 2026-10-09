# Module 10 — Exercise (Playbook ဖြင့် Missions ဖြတ်ခြင်း)

## E1. Mission ၅ ခု ဆက်တိုက် ဖြတ်ပါ
1. Missions စာရင်းမှ ပထမဆုံး mission ၅ ခု ကို အစဉ်အတိုင်း run ပါ
2. တစ်ခုချင်းအတွက် မှတ်တမ်း ပြုစုပါ —
   - Story ထဲက fault hint: ____
   - သုံးခဲ့သော commands: ____
   - ရလဒ် (success/retry): ____
   - Debrief သင်ခန်းစာ: ____

## E2. Command playbook drill
Terminal တွင် အောက်ပါ sequence ကို အချိန် ၅ မိနစ်အတွင်း run နိုင်အောင် လေ့ကျင့်ပါ —
1. `nvidia-smi`
2. `dcgmi diag -r 1`
3. `nvidia-smi nvlink -s`
4. `ibstat`
5. `nvsm show health`
6. `sinfo`

တစ်ကြောင်းချင်း ရဲ့ အဖြေကို တစ်စုံတစ်ရာ မမြင်ဘဲ ရှင်းပြနိုင်ရပါမည် (output ဖတ်ခြင်း ကျွမ်းကျင်မှု)။

## E3. Weak area ဖော်ထုတ်ခြင်း
Mission များ ဖြတ်ပြီးနောက် — ကိုယ် အကြိမ်ကြိမ် မှားခဲ့သော fault type / command family ကို
ရှာဖော်ပြီး ပြန်လည် လေ့ကျင့်ရန် plan တစ်ခု ရေးပါ
