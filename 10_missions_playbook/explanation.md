# Module 10 — 40 Missions Playbook

## 1. ပြဿနာ — လက်တွေ့ fault ကို ဘယ်လို လေ့ကျင့်မလဲ

Datacenter fault တစ်ခုကို ဖြေရှင်းနည်းကို စာဖတ်၍ သိရုံနှင့် မလုံလောက်ပါ —
တကယ့် cluster တွင် fault ထည့်၍ လေ့ကျင့်ရန် မဖြစ်နိုင်သလို မှားလျှင်လည်း ကုန်ကျစရိတ် ကြီးမားပါသည်။
ဖြေရှင်းရာတွင်လည်း command တစ်ခုတည်း မဟုတ်ဘဲ **အဆင့် အစဉ်** လိုက်ရန် လိုအပ်ပါသည်။
DC Lab Sim ၏ missions များက ဤ ကွာဟချက်ကို fault injection ပါသော guided scenario များဖြင့် ဖြည့်ပေးပါသည်။

## 2. Mission စနစ် နားလည်ခြင်း

DC Lab Sim ၏ missions များသည် **story hook + automatic fault injection + inline quiz + debrief**
ပါဝင်သော guided scenario များ ဖြစ်ပါသည် — စုစုပေါင်း **40 missions** (site v1.6.1 အရ)။ commands အရေအတွက်မှာ landing တွင် 229, Docs > Commands တွင် 214 commands / 17 categories ဟု ဖော်ပြထားပါသည်။ မစ်ရှင် ၄၀ ခု တစ်ခုချင်းစီ၏ အသေးစိတ် playbook ကို **Module 16 — Mission-by-Mission Playbooks** တွင် ကြည့်ပါ။

```
Story hook (ဖြစ်ရပ် ဇာတ်လမ်း)  →  fault ကို automatic inject
        ↓                              ↓
Debrief (သင်ခန်းစာ)  ←  inline quiz  ←  သင် ဖြေရှင်း (commands)
```

Scenario များသည် **sandboxed isolation** — mission တစ်ခုချင်းစီ၏ fault က အခြား mission/cluster ကို မထိခိုက်ပါ။

## 3. Mission အမျိုးအစား ၄ မျိုး ⭐

| အမျိုးအစား | လုပ်ဆောင်ရမည့် သဘော | ဥပမာ |
|---|---|---|
| **Bring-up** | စက်အသစ် တပ်ဆင်/အတည်ပြု | "The Midnight Deployment" |
| **Validate** | cluster ကျန်းမာရေး စစ်ဆေး | burn-in, benchmark |
| **Troubleshoot** | fault ရှာ/ဖြေရှင်း | XID, ECC, thermal |
| **Optimize** | performance/setting ပြင် | power cap, config |

Mission အမျိုးအစား ပေါ် မူတည်၍ သုံးရမည့် tool နှင့် အဆင့် အစဉ် ကွဲပါမည် —
Troubleshoot တွင် အနည်းဆုံး invasive နည်းမှ စခြင်း၊ Optimize တွင် baseline တိုင်းတာခြင်း ပါဝင်ပါသည်။

## 4. Story Hook ဖတ်နည်း — အရိပ်အမြွက် ⭐

ဇာတ်လမ်း စာသားထဲက **အရိပ်အမြွက်** က fault type ကို ဖော်ပြပါသည် —

| ဇာတ်လမ်း အရိပ်အမြွက် | ခန့်မှန်းရမည့် fault |
|---|---|
| "after shipping" | shipping damage / reseat လိုအပ်နိုင် |
| "stopped responding" | XID 43 (hang) |
| "fallen off the bus" / "not visible" | XID 79 (bus/PCIe) |
| "running hot" | thermal |
| "memory errors" | ECC |

ဇာတ်လမ်း ဖတ်ပြီး **fault hypothesis ကြိုတင် ခန့်မှန်း**ပြီးမှ command စပါ —
ဒါက စာမေးပွဲနှင့် လက်တွေ့ ၂ ခုလုံးတွင် အမြန်ဆုံး လမ်းကြောင်း ဖြစ်ပါသည်။

## 5. Command Playbook — Family အလိုက် (229 commands မှ အဓိက) ⭐

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

Command family တစ်ခုချင်းစီ၏ အဓိပ္ပာယ် —

- **State ကြည့်** — GPU ၏ လက်ရှိ အနေအထား (ECC count, temperature, power) ကို ဖတ်ခြင်း
- **Cluster** — DCGM ဖြင့် node အများ၏ health/discovery/diagnostics စစ်ဆေးခြင်း
- **Fabric** — NVLink/NVSwitch နှင့် InfiniBand link အခြေအနေ စစ်ဆေးခြင်း
- **System/Physical** — BMC (ipmitool), hardware inventory (dmidecode), kernel log (dmesg) ဖတ်ခြင်း
- **Jobs** — Slurm node/job state ကြည့်ခြင်း
- **Remediate** — GPU reset, ECC error clear, power cap ပြောင်းခြင်း၊ fabric manager restart ခြင်း၊
  chassis power cycle ခြင်း — ပြန်လည် ထူထောင်ရေး လုပ်ဆောင်ချက်များ
- **Physical** — GPU/NVLink bridge ပြန်တပ်ခြင်း (reseat)၊ မရတော့ပါက RMA အတွက် မှတ်ခြင်း
- **Report** — support/RMA အတွက် diagnostic log bundle ထုတ်ခြင်း

## 6. Mission ဖြတ်ရာတွင် လိုက်နာရန်

1. **sequence ကို လေးစားပါ** — "နောက်ဆုံး အဆင့်" ကို မေးလေ့ရှိ — command တစ်ခုတည်း မဟုတ်
2. **quiz ကို အဖြေ မမြန်ပါနှင့်** — debrief က သင်ခန်းစာ ပေး — မှားလည်း ကောင်းပါသည်
3. **debrief ကို မှတ်စု လုပ်ပါ** — module note ထဲ ပြန်ထည့်
4. mission ပြီးလျှင် **"ဘာကြောင့် မှားခဲ့သလဲ"** ကို ပြန်စဉ်းစားပါ

⚠️ Remediation အဆင့် အစဉ် — software → physical → replace — ကို mission တိုင်းတွင်
ထိန်းသိမ်းပါ — fault ကို ဖြေရှင်းနိုင်ရုံ မဟုတ်ဘဲ **မှန်ကန်သော အဆင့် အစဉ်ဖြင့်** ဖြေရှင်းခြင်းကို
လေ့ကျင့်ခြင်း ဖြစ်ပါသည်။

## 7. Self-Check

1. Mission နှင့် Sandbox ကွာခြားချက် အဓိက ဘာလဲ?
2. "stopped responding" ဆိုလျှင် မည်သည့် XID ကို ခန့်မှန်းသနည်း?
3. Remediation အဆင့် အစဉ် ၃ ဆင့် ဘာတွေလဲ?
4. Story hook ဖတ်ပြီး command မစမီ ဘာ လုပ်သင့်သလဲ?

**အဖြေ** —
1) Mission = guided + scoring + inline quiz + debrief၊ Sandbox = လွတ်လပ်စွာ စမ်းသပ်၊ score/quiz မပါ။
2) XID 43 (GPU hang / stopped responding)။
3) software (reset/config) → physical (reseat) → replace (RMA)။
4) ဇာတ်လမ်း အရိပ်အမြွက်မှ fault hypothesis ကြိုတင် ခန့်မှန်းပါ။

## 8. References

- DC Lab Sim missions: https://www.dclabsim.com
- Source code: https://github.com/Seanbo5386/dc-lab-sim

## Technical review notes

- "40 missions / 229 commands" ဆိုသော အရေအတွက်များသည် DC Lab Sim product အပေါ်
  မူတည်ပြီး version အလိုက် ပြောင်းလဲနိုင် — needs verification (product update တိုင်းတွင် ပြန်စစ်ပါ)။
- Self-Check အဖြေ 1 ရှိ Mission vs Sandbox ကွာခြားချက်သည် DC Lab Sim ၏
  feature ဖော်ပြချက်အပေါ် အခြေခံပြီး tool version ပေါ် မူတည် — needs verification။
- Command playbook စာရင်းကို မူရင်း အတိုင်း ထိန်းသိမ်းထားပြီး family အလိုက် ရှင်းလင်းချက်သာ
  ဖြည့်စွက်ထားပါသည် — command အသစ် မထည့်ပါ။
