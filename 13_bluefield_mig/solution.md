# Module 13 — Solution

## E1
1. `nvidia-smi -mig 1` → `nvidia-smi mig -cgi 9,9 -C` → `nvidia-smi mig -lgi` → `nvidia-smi mig -dgi -gi 0`
2. ① mode enable (`-mig 1`) ② instance ဖန်တီး (`mig -cgi ... -C`) — ① ပြီးမှ ② လုပ်မှ workload ထည့်နိုင်

## E2
1. Embedded/DPU mode (ARM က infrastructure ကိုင်), NIC mode (NIC ကဲ့သို့), Separated host mode
2. ဥပမာ — storage/network function ကို DPU က ကိုင်လိုက်လျှင် CPU က training အတွက် ပိုလွတ်

## E3
- **MIG** — hardware isolation လိုအပ်သော inference service များ — QoS တည်ငြိမ်
- time-slicing ဆို isolation မရ — vGPU က license လို
