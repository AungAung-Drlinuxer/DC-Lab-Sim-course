# Module 08 — Exercise

## E1. Slurm state
1. `sinfo` run ပြီး — node အခြေအနေ စာရင်း ဖြည့်ပါ
2. Workload (Training) apply ပြီး `sinfo` ပြန် run — state ပြောင်း/မပြောင်း

## E2. Workload profiling
1. Workload ၄ မျိုး (Idle/Inference/Training/Stress) တစ်ခုချင်း apply ပါ
2. တစ်ခုချင်းအတွက် `nvidia-smi` ဖြင့် Util/Mem/Pwr/Temp မှတ်ပြီး ဇယား ဖြည့်ပါ
3. မည်သည့် workload တွင် thermal အနီးစပ်ဆုံး လဲ ဆုံးဖြတ်ပါ

## E3. Diagnosis scenario
"Training job က မြန်နှေးနေတယ်၊ GPU util က 40% ပဲ ရှိတယ်" —
① စစ်ဆေးမည့် command ၃ ခု စီစဉ်ပြီး ② ဖြစ်နိုင်ချေ ၃ ခု စာရင်းပြုစုပါ
