# Module 10 — 32 Missions Playbook

## 1. Mission စနစ် နားလည်ခြင်း

DC Lab Sim ၏ missions များသည် **story hook + automatic fault injection + inline quiz + debrief**
ပါဝင်သော guided scenario များ ဖြစ်ပါသည် — စုစုပေါင်း 32 missions, 229 commands။

```
Story hook (ဖြစ်ရပ် ဇာတ်လမ်း)  →  fault ကို automatic inject
        ↓                              ↓
Debrief (သင်ခန်းစာ)  ←  inline quiz  ←  သင် ဖြေရှင်း (commands)
```

Scenario များသည် **sandboxed isolation** — mission တစ်ခုချင်းစီ၏ fault က အခြား mission/cluster ကို မထိခိုက်ပါ။

## 2. Mission အမျိုးအစား ၄ မျိုး ⭐

| အမျိုးအစား | လုပ်ဆောင်ရမည့် သဘော | ဥပမာ |
|---|---|---|
| **Bring-up** | စက်အသစ် တပ်ဆင်/အတည်ပြု | "The Midnight Deployment" |
| **Validate** | cluster ကျန်းမာရေး စစ်ဆေး | burn-in, benchmark |
| **Troubleshoot** | fault ရှာ/ဖြေရှင်း | XID, ECC, thermal |
| **Optimize** | performance/setting ပြင် | power cap, config |

## 3. Story Hook ဖတ်နည်း — အရိပ်အမြွက် ⭐

ဇာတ်လမ်း စာသားထဲက **အရိပ်အမြွက်** က fault type ကို ဖော်ပြပါသည် —
- "after shipping" → shipping damage / reseat လိုအပ်နိုင်
- "stopped responding" → XID 43 (hang)
- "fallen off the bus" / "not visible" → XID 79 (bus/PCIe)
- "running hot" → thermal
- "memory errors" → ECC

ဇာတ်လမ်း ဖတ်ပြီး **fault hypothesis ကြိုတင် ခန့်မှန်း**ပြီးမှ command စပါ —
ဒါက စာမေးပွဲနှင့် လက်တွေ့ ၂ ခုလုံးတွင် အမြန်ဆုံး လမ်းကြောင်း ဖြစ်ပါသည်။

## 4. Command Playbook — Family အလိုက် (229 commands မှ အဓိက) ⭐

```
[State ကြည့်]      nvidia-smi [-q|-d ECC|TEMPERATURE|POWER], nvsm show health
[Cluster]          dcgmi discovery|dmon|diag|health
[Fabric]           nvidia-smi nvlink|topo, ibstat
[System/Physical]  ipmitool, dmidecode, dmesg
[Jobs]             sinfo
[Remediate]        nvidia-smi --gpu-reset, --reset-ecc-errors, -pl,
                   systemctl restart nvidia-fabricmanager, ipmitool chassis power cycle
[Physical]         Reseat GPU / NVLink bridge / Mark for RMA
[Report]           nvidia-bug-report.sh
```

## 5. Mission ဖြတ်ရာတွင် လိုက်နာရန်

1. **sequence ကို လေးစားပါ** — "နောက်ဆုံး အဆင့်" ကို မေးလေ့ရှိ — command တစ်ခုတည်း မဟုတ်
2. **quiz ကို အဖြေ မမြန်ပါနှင့်** — debrief က သင်ခန်းစာ ပေး — မှားလည်း ကောင်းပါသည်
3. **debrief ကို မှတ်စု လုပ်ပါ** — module note ထဲ ပြန်ထည့်
4. mission ပြီးလျှင် **"ဘာကြောင့် မှားခဲ့သလဲ"** ကို ပြန်စဉ်းစားပါ

## 6. Self-Check

1. Mission နှင့် Sandbox ကွာခြားချက် အဓိက?
2. "stopped responding" ဆိုလျှင် မည်သည့် XID ကို ခန့်မှန်းသနည်း?
3. Remediation အဆင့် အစဉ် ၃ ဆင့်?

**အဖြေ** — 1) guided+scoring+quiz vs free/no-score · 2) XID 43 · 3) software → physical → replace

## 7. References

- DC Lab Sim missions: https://www.dclabsim.com
- Source code: https://github.com/Seanbo5386/dc-lab-sim
