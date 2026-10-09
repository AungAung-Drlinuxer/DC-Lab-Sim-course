# Module 01 — Exercise

## E1. Output ဖတ်လေ့ကျင့်ခြင်း
1. Terminal တွင် `nvidia-smi` run ပါ — GPU 0-7 အတွက် Temp, Pwr, Mem, Util ကို ဇယားဖြင့် မှတ်တမ်းတင်ပါ
2. `nvidia-smi -q` run ပြီး — Persistence Mode, Compute Mode, Driver Version, PCIe Gen ကို ရှာဖော်ပါ
3. `nvidia-smi topo -m` run ပြီး — GPU 0 နှင့် GPU 1 ကြား connection အမျိုးအစား ဘာလဲ ပြောပါ

## E2. Workload ၄ မျိုး နှိုင်းယှဉ်ခြင်း
Sandbox → **Simulate Workloads** ဖြင့် အောက်ပါ ၄ မျိုး တစ်ခုချင်း apply ပြီး `nvidia-smi` output မှတ်ပါ —
1. Idle (5%)
2. Inference (60%)
3. Training (95%)
4. Stress Test (100%)

Util/Mem/Pwr/Temp အတွက် ဇယား ဖြည့်ပါ။

## E3. Fault ထည့်ပြီး ဖတ်ခြင်း
1. Sandbox တွင် GPU 2 ၌ **Thermal Issue** inject ပါ
2. `nvidia-smi -q -d TEMPERATURE` run ပြီး အပူချိန် ပြောင်းလဲမှု မှတ်ပါ
3. `nvidia-smi` default output ထဲက Perf column ဘာ ပြောင်းသွားသနည်း ကြည့်ပါ
4. **Reset to healthy** ပြုလုပ်ပြီး ပုံမှန် ပြန်ကြောင်း အတည်ပြုပါ
