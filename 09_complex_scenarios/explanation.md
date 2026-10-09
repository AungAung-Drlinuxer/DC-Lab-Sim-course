# Module 09 — Complex Multi-Symptom Scenarios

## 1. ဘာကြောင့် complex scenario လေ့ကျင့်ရသလဲ

တကယ့် datacenter တွင် ပြဿနာများသည် **symptom တစ်ခုတည်း မပါ**လေ့ ရှိပါ —
XID ရှိနေခြင်း + အပူတက်ခြင်း + performance ကျခြင်း ပေါင်းစု ဖြစ်တတ်ပါသည်။
DC Lab Sim ၏ **Complex Training Scenarios** က multi-symptom failure များကို
စနစ်တကျ ဖြေရှင်းနည်း လေ့ကျင့်ပေးပါသည်။

## 2. Scenario ၄ ခု — သဘောသဘာဝ ⭐

### ① GPU Hang (XID 43)
- **symptom** — GPU က တုံ့ပြန်ခြင်း ရပ်; job ကပ်နေ; XID 43 log
- **root cause** — kernel ကြာခြင်း/driver/clock — software-level အများ
- **ဖြေရှင်း** — `nvidia-smi --gpu-reset` → မရပါက node reboot → ထပ်ဖြစ်ပါက replace

### ② Bus Reset (XID 79)
- **symptom** — `GPU has fallen off the bus`; `nvidia-smi` က GPU ကို မမြင် (missing)
- **root cause** — PCIe link/power/hardware
- **ဖြေရှင်း** — reseat GPU → power cycle → မကောင်းပါက RMA

### ③ Thermal Alert (All GPUs 90-100°C)
- **symptom** — GPU အားလုံး အပူများ; throttling (Perf ကျ); job နှေး
- **root cause** — cooling/airflow/room/fan — GPU တစ်လုံးတည်း မဟုတ်
- **ဖြေရှင်း** — workload ရပ် → airflow/CRAC/fan စစ် → ပြန်အေးမှ workload ပြန်စ

### ④ Severe ECC (Uncorrectable)
- **symptom** — uncorrectable ECC error များ; XID 48/94; job corruption
- **root cause** — HBM hardware ပျက်စီး
- **ဖြေရှင်း** — ခွဲထုတ် → အတည်ပြု → GPU replace/RMA

## 3. Systematic Workflow ⭐ (စာမေးပွဲ D5 အတွက် အဓိက)

```
① Gather     symptom အားလုံး စု — dmesg, nvidia-smi, dcgmi, nvsm, ipmitool
② Hypothesize ဖြစ်နိုင်ချေ စာရင်း (power? thermal? HW? SW?)
③ Test       တစ်ခုချင်း စစ်ဆေး — hypothesis ကို ဖျက်/အတည်
④ Fix        အနည်းဆုံး invasive နည်းမှ စ (config → reset → reseat → replace)
⑤ Verify     ပြန်ကျန်းမာကြောင်း tool ဖြင့် သက်သေပြ
⑥ Document   nvidia-bug-report.sh — report/RMA
```

⚠️ **Fix အဆင့် အစဉ်** — software (reset/config) → physical (reseat) → replace (RMA) —
ချက်ချင်း hardware လဲခြင်းသည် နောက်ဆုံး အဆင့်သာ ဖြစ်ပါသည်။

## 4. Symptom ခွဲခြားခြင်း — မေးခွန်း ၃ ခု ⭐

1. **scope** — GPU တစ်လုံးလား / အားလုံးလား / node တစ်ခုလုံးလား / cluster လား?
2. **onset** — ရုတ်တရက်လား (fault) / တဖြည်းဖြည်းလား (aging/thermal)?
3. **correlation** — workload ပြောင်းမှ ဖြစ်လား / firmware update ပြီးမှ ဖြစ်လား?

## 5. Self-Check

1. XID 43 နှင့် XID 79 ၏ ဖြေရှင်းနည်း ပထမ အဆင့် မတူရခြင်း အကြောင်းရင်း?
2. GPU အားလုံး ပူနေလျှင် မည်သည့် scope ကို စဉ်းစားသနည်း?
3. "အနည်းဆုံး invasive နည်းမှ စ" ဆိုတာ ဘာကို ဆိုလိုသလဲ?

**အဖြေ** — 1) 43 = software/reset လုံလောက်နိုင်, 79 = hardware/bus — reset မလုံလောက် · 2) cooling/system scope (node/room) · 3) config → reset → reseat → replace အစဉ်

## 6. References

- XID errors: https://docs.nvidia.com/deploy/xid-errors/
- DCGM diagnostics: https://docs.nvidia.com/datacenter/dcgm/
