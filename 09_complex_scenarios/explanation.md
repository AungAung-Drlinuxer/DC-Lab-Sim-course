# Module 09 — Complex Multi-Symptom Scenarios

## 1. ပြဿနာ — symptom တစ်ခုတည်းနှင့် လာသော ပြဿနာ နည်းပါသည်

တကယ့် datacenter တွင် ပြဿနာများသည် **symptom တစ်ခုတည်း မပါ**လေ့ ရှိပါ —
XID ရှိနေခြင်း + အပူတက်ခြင်း + performance ကျခြင်း ပေါင်းစု ဖြစ်တတ်ပါသည်။
symptom တစ်ခုချင်းစီကို တစ်ခုချင်း ဖြေရှင်းပါက အချိန် ကုန်ပြီး root cause ကို လွဲနိုင်ပါသည် —
ဒါကြောင့် multi-symptom failure များကို **စနစ်တကျ** ချဉ်းကပ်နည်း လိုအပ်ပါသည်။
DC Lab Sim ၏ **Complex Training Scenarios** က ဤ ချဉ်းကပ်နည်းကို လေ့ကျင့်ပေးပါသည်။

## 2. Scenario ၄ ခု — သဘောသဘာဝ ⭐

### ① GPU Hang (XID 43)
- **symptom** — GPU က တုံ့ပြန်ခြင်း ရပ်; job ကပ်နေ; XID 43 log
- **root cause** — application/software-level အများစု (kernel ကြာခြင်း/driver/clock လည်း ဖြစ်နိုင်)
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
- **symptom** — uncorrectable ECC error များ; XID 48 (double-bit) နှင့် error containment ရှိသော GPU များတွင် XID 94/95 (contained/uncontained); job corruption
- **root cause** — HBM/memory hardware ပျက်စီး
- **ဖြေရှင်း** — ခွဲထုတ် (isolate) → အတည်ပြု → error ဆက်လက် များပါက GPU replace/RMA

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

Workflow အဆင့် ၆ ဆင့်လုံးကို မကျော်ဘဲ လိုက်ပါ — symptom စုခြင်း (①) ကို ကျော်ပြီး
fix ချက်ချင်း လုပ်ခြင်းသည် အဖြစ်များဆုံး မှားယွင်းမှု ဖြစ်ပါသည်။

## 4. Symptom ခွဲခြားခြင်း — မေးခွန်း ၃ ခု ⭐

1. **scope** — GPU တစ်လုံးလား / အားလုံးလား / node တစ်ခုလုံးလား / cluster လား?
2. **onset** — ရုတ်တရက်လား (fault) / တဖြည်းဖြည်းလား (aging/thermal)?
3. **correlation** — workload ပြောင်းမှ ဖြစ်လား / firmware update ပြီးမှ ဖြစ်လား?

ဤ ၃ ခုကို ဖြေနိုင်ပါက hypothesis စာရင်းသည် အလိုအလျောက် ကျဉ်းမြောင်းသွားပါမည် —
ဥပမာ GPU အားလုံး တစ်ချိန်တည်း ပူနေပါက GPU တစ်လုံး၏ ချို့ယွင်းမှု မဟုတ်ဘဲ
cooling/airflow/room-level (node/room scope) ကို စဉ်းစားရပါမည်။

## 5. လုပ်ငန်းခွင် သတိပြုရန် အချက်များ

- **ရှုပ်ထွေးသော symptom များတွင် ပထမ မမှန်းပါနှင့်** — §3 ၏ Gather → Hypothesize အဆင့်ကို
  မလုပ်ဘဲ ဖြေရှင်းချက် ရွေးပါက root cause လွဲတတ်ပါသည်။
- **reset/reboot သည် symptom ကို ဖုံးကွယ်နိုင်ပါသည်** — ပြန်ဖြစ်ပါက log (dmesg, XID)
  ကို reset မလုပ်မီ အရင် သိမ်းဆည်းထားပါ။
- **Document အဆင့် မကျန်စေပါနှင့်** — `nvidia-bug-report.sh` output မပါဘဲ RMA တင်ပါက
  ထပ်ခါထပ်ခါ အချက်အလက် တောင်းခံခံရနိုင်ပါသည်။
- **XID 43 နှင့် XID 79 ကို မရောပါနှင့်** — 43 သည် processing ရပ်ခြင်း (software-level
  အများစု)၊ 79 သည် PCIe bus ပေါ်မှ GPU ပျောက်သွားခြင်း (hardware/link) ဖြစ်ပြီး
  ဖြေရှင်းနည်း အစ မတူပါ။

## 6. Self-Check

1. XID 43 နှင့် XID 79 ၏ ဖြေရှင်းနည်း ပထမ အဆင့် မတူရခြင်း အကြောင်းရင်း ဘာလဲ?
2. GPU အားလုံး ပူနေလျှင် မည်သည့် scope ကို စဉ်းစားသနည်း?
3. "အနည်းဆုံး invasive နည်းမှ စ" ဆိုတာ ဘာကို ဆိုလိုသလဲ?
4. Systematic workflow တွင် ဖြေရှင်းချက် မလုပ်မီ ပထမ လုပ်ရမည့် အဆင့် ၂ ခု ဘာတွေလဲ?

**အဖြေ** —
1) XID 43 = software-level (reset ဖြင့် လုံလောက်နိုင်)၊ XID 79 = hardware/bus — reset ဖြင့် မလုံလောက်ပါ။
2) cooling/system scope (node/room level — airflow, CRAC, fan)။
3) config → reset → reseat → replace အစဉ် — ပြင်းထန်သော နည်းလမ်းကို နောက်ဆုံးမှ သုံးခြင်း။
4) ① Gather (symptom အားလုံး စု) နှင့် ② Hypothesize (ဖြစ်နိုင်ချေ စာရင်း)။

## 7. References

- XID errors: https://docs.nvidia.com/deploy/xid-errors/
- DCGM diagnostics: https://docs.nvidia.com/datacenter/dcgm/

## Technical review notes

- XID 43 ကို NVIDIA XID catalog အရ "GPU stopped processing" ဟု ဖော်ပြပြီး အများအားဖြင့်
  user application ၏ software-induced fault ကြောင့် log ကျခြင်းဖြစ်ပြီး GPU ကိုယ်တိုင်
  ကျန်းမာနေဆဲ ဖြစ်နိုင်ပါသည် (application restart ဖြင့် လုံလောက်နိုင်) — မူရင်း၏
  reset → reboot → replace အစဉ်ကို GPU တုံ့ပြန်မှု လုံးဝ ရပ်နေပါက ဆက်လက် လုပ်ဆောင်ရန်
  အဆင့်မြှင့် လမ်းကြောင်းအဖြစ် ပြင်ဆင် ဖော်ပြထားပါသည် (ပြင်ဆင်မှု ပြုလုပ်ပြီး)။
- XID 79 ("GPU has fallen off the bus" — PCIe link/power/hardware; reseat → power cycle → RMA) ကို
  NVIDIA မှတ်တမ်းဖြင့် အတည်ပြုပြီး ဖြစ်ပါသည်။
- XID 48 = Double Bit ECC Error (uncorrectable) — အတည်ပြုပြီး။ XID 94/95 သည် A100 မှ စ၍
  error containment ရှိသော GPU များ၏ contained/uncontained memory error ဖြစ်ပြီး XID 48 နှင့်
  သဘောသဘာဝ မတူပါ — XID 94 တွင် ထိခိုက်သော application ကို restart လုပ်ရန် လိုအပ်ပြီး
  အခြား application များ ဆက်လက် အလုပ်လုပ်နိုင်ဆဲ ဖြစ်ပါသည် (ပြင်ဆင်မှု ပြုလုပ်ပြီး)။
- Scenario ① ၏ root cause စာရင်း (kernel delay/driver/clock) သည် ဖြစ်နိုင်ချေ အနေဖြင့်
  ဖော်ပြခြင်းဖြစ်ပြီး အခြေအနေ တစ်ခုချင်းစီတွင် အတည်ပြုရန် လိုအပ် — needs verification။
