# Module 06 — Exercise

## E1. NVLink topology
1. Architecture ကို DGX A100 သတ်မှတ်ပြီး `nvidia-smi nvlink -s` run ပါ — link အရေအတွက် ရေတွက်ပါ
2. `nvidia-smi topo -m` run ပြီး — GPU 0-7 matrix မှ NVLink အုပ်စုများ ဖော်ထုတ်ပါ
3. Architecture ကို DGX H100 ပြောင်းပြီး `nvidia-smi nvlink -s` ပြန် run — ကွာခြားချက် မှတ်ပါ

## E2. NVLink fault
1. Sandbox တွင် **NVLink Down** inject ပါ
2. `nvidia-smi nvlink -s` — ဘယ် link ဘာ state ဖြစ်သွားသနည်း
3. `nvidia-smi nvlink -e` — error counter များ ကြည့်ပါ
4. remediation အဖြစ် `systemctl restart nvidia-fabricmanager` စဉ်းစားပြီး —
   Physical Action **Reseat NVLink bridge** ကိုပါ စမ်းသပ်ပါ
5. Reset to healthy ပြီးနောက် link ပြန် Active ဖြစ်ကြောင်း အတည်ပြုပါ

## E3. InfiniBand
1. `ibstat` run ပြီး — port state, rate မှတ်တမ်းတင်ပါ
2. Dashboard ၏ **InfiniBand Fabric** tab နှင့် နှိုင်းယှဉ်ပါ
