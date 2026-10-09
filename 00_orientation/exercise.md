# Module 00 — Exercise (DC Lab Sim ထဲ လုပ်ပါ)

## E1. Dashboard ရင်းနှီးမှု
1. dclabsim.com ကို ဖွင့်ပြီး **Enter Virtual Datacenter** ဝင်ပါ
2. Cluster Health တွင် — Nodes Online? GPUs Healthy? InfiniBand? BCM HA? မှတ်တမ်းတင်ပါ
3. `dgx-00` ကို ရွေးပြီး — Node Details တွင် CPU cores, RAM, Driver, CUDA, OS, Slurm state များ ဖြည့်ပါ

## E2. Architecture ပြောင်းကြည့်ခြင်း
1. Architecture dropdown ကို DGX A100 → DGX B200 ပြောင်းပါ
2. ပြောင်းပြီးနောက် GPU memory, NVLink topology တွင် ဘာတွေ ပြောင်းသွားသနည်း မှတ်တမ်းတင်ပါ
3. NVLink Topology tab ကို ဖွင့်ပြီး link အရေအတွက် ရေတွက်ပါ

## E3. Terminal ပထမဆုံး command များ
Terminal tab တွင် အောက်ပါ command များ တစ်ကြောင်းချင်း run ပြီး output အနှစ်ချုပ် ရေးပါ —
1. `nvidia-smi`
2. `nvidia-smi -q -d TEMPERATURE`
3. `sinfo`
4. `ipmitool sensor list`

## E4. Sandbox ပထမဆုံး fault
1. Sandbox tab သို့ သွားပါ — node `dgx-00`, `GPU 0` ရွေးပါ
2. **XID Error** ကို inject ပါ (Submit)
3. Terminal တွင် `dmesg | grep -i xid` run ပြီး ဘာတွေ့သနည်း မှတ်ပါ
4. **Clear All / Reset to healthy** ဖြင့် ပြန်ပြင်ပြီး `nvidia-smi` ဖြင့် ကျန်းမာပြန်ကြောင်း အတည်ပြုပါ
