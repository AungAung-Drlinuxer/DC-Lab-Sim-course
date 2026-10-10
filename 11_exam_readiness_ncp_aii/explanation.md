# Module 11 — Exam Readiness (NCP-AII)

## 1. ပြဿနာ — အကြောင်းအရာ တတ်ရုံနှင့် စာမေးပွဲ မအောင်နိုင်ပါ

NCP-AII စာမေးပွဲသည် 120 မိနစ်အတွင်း မေးခွန်း ၇၀-၇၅ ခု — မေးခွန်း ၁ ခုလျှင် ၁.၆-၁.၇ မိနစ်သာ
ရှိပါသည်။ အကြောင်းအရာ သိရုံနှင့် မလုံလောက်ဘဲ —

- domain အလိုက် **အလေးချိန်** မသိပါက အချိန် မျှမမျှ သုံးမိပါမည်
- "next step" / "EXCEPT" ကဲ့သို့ **trap မေးခွန်း** ပုံစံများကို မကျွမ်းကျင်ပါက သိထားသော
  အကြောင်းအရာများပင် မှားဖြေမိပါမည်
- platform တစ်ခုချင်းစီ၏ procedure ကို universal ဟု မှားယူပါက ဖြေရွေး လွဲပါမည်

ဤ chapter သည် blueprint အလေးချိန်၊ exam format၊ အဖြစ်များသော trap များ၊
အချိန် စီမံခန့်ခွဲမှုနှင့် နောက်ဆုံး အပတ် အစီအစဉ်ကို ဖုံးလွှမ်းပါသည်။

## 2. Blueprint ပြန်လည် သုံးသပ်ခြင်း ⭐

| Domain | Weight | ပါဝင်သော အဓိက အကြောင်းအရာ |
|---|---|---|
| **D1 Systems & Server Bring-Up** | **31%** | deployment sequence, AI factory topology, BMC/OOB/TPM, firmware, power/cooling validate, cable/transceiver, hardware verify |
| **D2 Physical Layer** | 5% | BlueField DPU platform, MIG (AI/HPC) |
| **D3 Control Plane** | 19% | BCM (+HA), OS install, Slurm/Enroot/Pyxis, GPU/DOCA drivers, container toolkit, Docker+GPU, NGC CLI |
| **D4 Cluster Test & Verification** | **33%** | stress test, HPL, NCCL (+NVLink Switch), cable signal, FW verify (switch/BlueField/transceiver), ClusterKit, NCCL/HPL/NeMo burn-in, storage test |
| **D5 Troubleshooting & Optimization** | 12% | hardware fault identify/replace (GPU/fan/NIC/PSU), AMD/Intel optimization, storage optimization |

📌 D1 + D4 = **64%** — ဤ ၂ domain ကို အချိန် အများဆုံး ပေးပါ။
သို့သော် D2 (5%) ကဲ့သို့ ငယ်သော domain များကိုလည်း လုံးဝ မကျန်စေပါနှင့်။

## 3. Exam အချက်အလက်

- 70-75 မေးခွန်း / 120 မိနစ် ≈ မေးခွန်း ၁ ခုလျှင် 1.6-1.7 မိနစ်
- Multiple choice + multiple response
- Certiverse (remote proctored)၊ $400၊ 2 နှစ် သက်တမ်း
- အောင်မှတ် မထုတ်ပြန် — 85%+ practice ရအောင် လေ့ကျင့်ပါ (margin ထား)

## 4. Top Exam Traps ⭐⭐

1. **Platform-specific procedure** — DGX A100 bring-up = HGX B200 bring-up မဟုတ်
   — ဖြေရွေးရာတွင် platform တစ်ခု၏ runbook ကို universal ဟု ပြလျှင် သတိပြု
2. **"Enabled" ≠ "Usable"** — MIG mode on ထားခြင်း ≠ instance ဖန်တီးပြီး အသုံးပြုနိုင်ခြင်း
   — ဒုတိယ အဆင့်ကို သက်သေပြသော command ကို ရွေးရမည်
3. **Sequence က ကိုယ်တိုင် အမေး** — "next step" / "prerequisite" ပုံစံ
4. **Tool output ဖတ်ခြင်း** — command ရွေးရုံ မဟုတ် — `nvidia-smi`/NVSM/DCGM/dmesg
   output က ဘာ ပြောနေသလဲ (Xid/SXid signature အပါအဝင်)
5. **Tool တစ်ခုချင်း ၏ သက်သေပြမှု** —
   HPL = sustained compute/thermals · NCCL = collective bandwidth ·
   ClusterKit = node-to-node · NeMo burn-in = end-to-end (storage/data ပါ)
   — ဇယားတွင် မှတ်ပြီး ကွဲအောင် ထားပါ
6. **ငယ်သော domain မကျန်စေနှင့်** — D2 (5%) က BlueField + MIG — ရှုပ်ထွေးပြီး ရမှတ် လွယ်

## 5. Time Management ⭐

```
0-10 မိနစ်     မေးခွန်း အားလုံး ဖြတ်ကြည့် — သေချာသော ချက်ချင်း ဖြေ
10-100 မိနစ်   ကျန် မေးခွန်းများ (1.5 မိနစ်/ခု budget)၊ မသေချာပါက flag
100-120 မိနစ်  flag ပြန်စစ် — multiple response တွင် option အားလုံး ပြန်ဖတ်
```

⚠️ "EXCEPT / NOT / BEST / FIRST" ကဲ့သို့ စကားလုံးများကို အထူး ဂရုပြုဖတ်ပါ —
မေးခွန်း အများစု မှားခြင်းမှာ ဖတ်ရှုမှု မြန်ခြင်းကြောင့် ဖြစ်ပါသည်။

## 6. နောက်ဆုံး အပတ် Plan ⭐

| ရက် | လုပ်ရန် |
|---|---|
| Day 1-2 | D1 (31%) ပြန်ဖတ် + missions bring-up |
| Day 3-4 | D4 (33%) ပြန်ဖတ် + validate/burn-in missions |
| Day 5 | D3 (19%) + D5 (12%) |
| Day 6 | D2 (5%) + weak areas + quiz အပြည့် |
| Day 7 | Practice exam အပြည့် (120 မိနစ်) + အားနည်းချက် ပြင် |

## 7. စာမေးပွဲ နေ့ Checklist

- [ ] ID အသင့်၊ တိတ်ဆိတ်သော အခန်း၊ ရှင်းလင်းသော စားပွဲ
- [ ] Certiverse platform test ကြိုတင် လုပ်ပြီး
- [ ] Top traps (Module 11 §4) ဖြေဆိုမီ ပြန်ဖတ်
- [ ] Tool-purposes ဇယား (HPL/NCCL/ClusterKit/NeMo) မှတ်မိ
- [ ] Time budget စီမံခန့်ခွဲမှု ကြိုတင် စီစဉ်ပြီး

## 8. NCA-AIIO မှ NCP-AII သို့

NCA-AIIO (associate) က ဖြေပြီးပါက — ဤ NCP-AII က နောက် အဆင့် သင့်ပါသည် —
associate က "ဘာလဲ" မေး၊ professional က "ဘယ်လို တပ်/အတည်ပြု/ပြင်မလဲ" မေးပါသည်။
DC Lab Sim က ထိုကွာခြားချက်ကို လက်တွေ့ ဖြည့်ပေးပါသည်။

## 9. Self-Check

1. D1 + D4 ပေါင်းလျှင် exam weight မည်မျှ ရှိပြီး ဘာကြောင့် အရေးကြီးသလဲ?
2. မေးခွန်း ၁ ခုအတွက် အချိန် budget ပျမ်းမျှ မည်မျှလဲ?
3. "MIG mode on ထားသည်" ဆိုခြင်းက instance အသုံးပြုနိုင်ပြီဟု ဆိုလိုသလား?
4. HPL, NCCL, ClusterKit, NeMo burn-in တို့ တစ်ခုချင်းစီ သက်သေပြသော အချက် ဘာလဲ?

**အဖြေ** —
1) 64% — မေးခွန်း အများစု ဤ ၂ domain မှ လာပါသည်။
2) ခန့်မှန်းချက်အားဖြင့် 1.6-1.7 မိနစ် (70-75 ခု / 120 မိနစ်)။
3) မဆိုလိုပါ — "Enabled" ≠ "Usable"; instance ဖန်တီးပြီး အသုံးပြုနိုင်ကြောင်း သက်သေပြရန် လိုပါသည်။
4) HPL = sustained compute/thermal၊ NCCL = GPU/node ကြား collective bandwidth၊
   ClusterKit = node-to-node၊ NeMo burn-in = end-to-end training path (storage/data အပါအဝင်)။

## 10. References

- NCP-AII official: https://www.nvidia.com/en-us/learn/certification/ai-infrastructure-professional/
- Exam Study Guide PDF (NVIDIA)
- Certiverse: https://certiverse.nvidia.com
- NCP-AIN (networking) — https://www.nvidia.com/en-us/learning/

## Technical review notes

- §3 ရှိ exam format အချက်အလက်များ (70-75 မေးခွန်း/120 မိနစ်၊ $400၊ Certiverse remote
  proctoring၊ 2 နှစ် သက်တမ်း၊ အောင်မှတ် မထုတ်ပြန်) ကို အများပြည်သူ ရရှိနိုင်သော
  အရင်းအမြစ်များဖြင့် အတည်ပြုပြီး ဖြစ်သော်လည်း ဈေးနှုန်း/format သည် အချိန်နှင့်အမျှ
  ပြောင်းလဲနိုင် — ဖြေဆိုမီ NVIDIA official page တွင် ပြန်စစ်ပါ။
- §2 ရှိ blueprint domain weights (D1 31%, D4 33%, D3 19%, D5 12%, D2 5%) ကို တတိယ
  ပါတီ အရင်းအမြစ်များနှင့် ကိုက်ညီသော်လည်း တရားဝင် Exam Study Guide PDF နှင့်
  နောက်ဆုံး version အတိုင်း ပြန်လည် အတည်ပြုရန် လိုအပ် — needs verification။
- Practice 85%+ ရမှ ဖြေဆိုရန် ဆိုသော အကြံပြုချက်သည် ကိုယ်ပိုင် အကြံပြုချက်ဖြစ်ပြီး
  NVIDIA ၏ တရားဝင် အချက်အလက် မဟုတ်ပါ (passing score မထုတ်ပြန်ပါ)။
