# Module 11 — Exam Readiness (NCP-AII)

## 1. Blueprint ပြန်လည် သုံးသပ်ခြင်း ⭐

| Domain | Weight | ပါဝင်သော အဓိက အကြောင်းအရာ |
|---|---|---|
| **D1 Systems & Server Bring-Up** | **31%** | deployment sequence, AI factory topology, BMC/OOB/TPM, firmware, power/cooling validate, cable/transceiver, hardware verify |
| **D2 Physical Layer** | 5% | BlueField DPU platform, MIG (AI/HPC) |
| **D3 Control Plane** | 19% | BCM (+HA), OS install, Slurm/Enroot/Pyxis, GPU/DOCA drivers, container toolkit, Docker+GPU, NGC CLI |
| **D4 Cluster Test & Verification** | **33%** | stress test, HPL, NCCL (+NVLink Switch), cable signal, FW verify (switch/BlueField/transceiver), ClusterKit, NCCL/HPL/NeMo burn-in, storage test |
| **D5 Troubleshooting & Optimization** | 12% | hardware fault identify/replace (GPU/fan/NIC/PSU), AMD/Intel optimization, storage optimization |

📌 D1 + D4 = **64%** — ဤ ၂ domain ကို အချိန် အများဆုံး ပေးပါ။

## 2. Exam အချက်အလက်

- 70-75 မေးခွန်း / 120 မိနစ် ≈ မေးခွန်း ၁ ခုလျှင် 1.6-1.7 မိနစ်
- Multiple choice + multiple response
- Certiverse (remote proctored)၊ $400၊ 2 နှစ် သက်တမ်း
- အောင်မှတ် မထုတ်ပြန် — 85%+ practice ရအောင် လေ့ကျင့်ပါ (margin ထား)

## 3. Top Exam Traps ⭐⭐

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

## 4. Time Management ⭐

```
0-10 မိနစ်     မေးခွန်း အားလုံး ဖြတ်ကြည့် — သေချာသော ချက်ချင်း ဖြေ
10-100 မိနစ်   ကျန် မေးခွန်းများ (1.5 မိနစ်/ခု budget)၊ မသေချာပါက flag
100-120 မိနစ်  flag ပြန်စစ် — multiple response တွင် option အားလုံး ပြန်ဖတ်
```

⚠️ "EXCEPT / NOT / BEST / FIRST" ကဲ့သို့ စကားလုံးများကို အထူး ဂရုပြုဖတ်ပါ —
မေးခွန်း အများစု မှားခြင်းမှာ ဖတ်ရှုမှု မြန်ခြင်းကြောင့် ဖြစ်ပါသည်။

## 5. နောက်ဆုံး အပတ် Plan ⭐

| ရက် | လုပ်ရန် |
|---|---|
| Day 1-2 | D1 (31%) ပြန်ဖတ် + missions bring-up |
| Day 3-4 | D4 (33%) ပြန်ဖတ် + validate/burn-in missions |
| Day 5 | D3 (19%) + D5 (12%) |
| Day 6 | D2 (5%) + weak areas + quiz အပြည့် |
| Day 7 | Practice exam အပြည့် (120 မိနစ်) + အားနည်းချက် ပြင် |

## 6. စာမေးပွဲ နေ့ Checklist

- [ ] ID အသင့်၊ တိတ်ဆိတ်သော အခန်း၊ ရှင်းလင်းသော စားပွဲ
- [ ] Certiverse platform test ကြိုတင် လုပ်ပြီး
- [ ] Top traps (Module 11 §3) ဖြေဆိုမီ ပြန်ဖတ်
- [ ] Tool-purposes ဇယား (HPL/NCCL/ClusterKit/NeMo) မှတ်မိ
- [ ] Time budget စီမံခန့်ခွဲမှု ကြိုတင် စီစဉ်ပြီး

## 7. NCA-AIIO မှ NCP-AII သို့

NCA-AIIO (associate) က ဖြေပြီးပါက — ဤ NCP-AII က နောက် အဆင့် သင့်ပါသည် —
associate က "ဘာလဲ" မေး၊ professional က "ဘယ်လို တပ်/အတည်ပြု/ပြင်မလဲ" မေးပါသည်။
DC Lab Sim က ထိုကွာခြားချက်ကို လက်တွေ့ ဖြည့်ပေးပါသည်။

## 8. References

- NCP-AII official: https://www.nvidia.com/en-us/learn/certification/ai-infrastructure-professional/
- Exam Study Guide PDF (NVIDIA)
- Certiverse: https://certiverse.nvidia.com
- NCP-AIN (networking) — https://www.nvidia.com/en-us/learning/
