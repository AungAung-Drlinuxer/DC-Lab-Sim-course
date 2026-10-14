# Module 16 — Exercise: Mission Drill Plan

## E1. Domain အစဉ်အတိုင်း Mission Run (ပထမ အကြိမ်)

Mission ၄၀ ကို explanation.md ၏ catalog အစဉ် (Domain 0 → Domain 5) အတိုင်း
တစ်ခုချင်း လုပ်ပါ — တစ်ခုချင်းစီအတွက် အောက်ပါ log ကို ဖြည့်ပါ။

| Mission # | Domain | ပထမ ချဉ်းကပ်မှု မှန်/မမှန် | အသုံးပြုခဲ့သော commands | လွဲခဲ့သော အဆင့် | မှတ်စရာ |
|---|---|---|---|---|---|
| 1 | 0 | | | | |
| 2 | 0 | | | | |
| ... | | | | | |

စည်းမျဉ်းများ —

1. playbook ကို **mission မစမီ မဖတ်ပါနှင့်** — ပထမ အကြိမ်တွင် ကိုယ်ပိုင်
   ချဉ်းကပ်မှုဖြင့် အရင် စမ်းပါ (E3 ကို ကြည့်)။
2. mission ပြီး/မပြီး ကို သာမက **မည်သည့် အဆင့်တွင် လွဲခဲ့သည်** ကို
   မှတ်ပါ — miss log က re-drill ရဲ့ အခြေခံ ဖြစ်သည်။
3. တစ်နေ့ ၅ မစ်ရှင်ထက် ပိုမလုပ်ပါနှင့် — အမှားကို ပြန်စဉ်းစားချိန် လိုအပ်သည်။

## E2. Miss Log အလိုက် Targeted Re-Drill

E1 ပြီးသောအခါ miss log ကို fault class အလိုက် အုပ်စုခွဲပါ —

- **XID/ECC miss များ** → Module 03/04 ပြန်ဖတ်ပြီး mission 36, 38 ပြန်လုပ်
- **Thermal/power miss** → Module 05 ပြန်ဖတ်ပြီး mission 30, 32 ပြန်လုပ်
- **NVLink/fabric miss** → Module 06 ပြန်ဖတ်ပြီး mission 12, 16, 26 ပြန်လုပ်
- **IB/physical miss** → Module 06/12 ပြန်ဖတ်ပြီး mission 14, 33, 40 ပြန်လုပ်
- **Slurm/container miss** → Module 08/14 ပြန်ဖတ်ပြီး mission 18, 24, 25 ပြန်လုပ်
- **NCCL/HPL/validation miss** → Module 15 ပြန်ဖတ်ပြီး mission 31, 35, 36
  ပြန်လုပ်
- **Bring-up sequence miss** → Module 10 ပြန်ဖတ်ပြီး mission 5, 7 ပြန်လုပ်

re-drill တွင်လည်း log အသစ် ထပ်ဖြည့်ပါ — ဒုတိယ အကြိမ်တွင်လည်း
လွဲပါက ထို fault class ကို သီးခြား အချိန်ပေး လေ့ကျင့်ပါ။

## E3. Scenario Drill ၃ ခု — playbook မဖတ်မီ ကိုယ်ပိုင် triage plan ရေးပါ

အောက်ပါ mission ၃ ခုအတွက် **playbook ကို မဖတ်မီ** ကိုယ်ပိုင် triage plan
ကို စာရွက်ပေါ်တွင် ရေးပါ — ပြီးမှ explanation.md ရှိ playbook နှင့်
နှိုင်းယှဉ်ပါ။ plan တစ်ခုချင်းတွင် အောက်ပါ ၄ ခု ပါရမည် —

1. ဖြစ်နိုင်ချေရှိသော fault class ၂–၃ ခု (priority အလိုက် စီ)
2. အသုံးပြုမည့် command အဆင့်စဉ် (၃–၆ ခု)
3. remediation အဆင့်စဉ် (fix order)
4. အောင်မြင်ကြောင်း အတည်ပြုမည့် နည်းလမ်း

### Drill A — Mission 16: The NVLink Mystery (advanced)

> Symptom: multi-GPU training က 8-GPU scaling မှ 4-GPU performance အထိ
> ရုတ်တရက် ကျသွားသည်။

ကိုယ်ပိုင် plan ရေးပါ — ဘယ် tool က အရင်? ဘယ် fix order? ဘယ်လို validate?

### Drill B — Mission 12: NVLink & Fabric Manager Activation (intermediate)

> Symptom: node အသစ် တစ်လုံးတွင် GPU ၈ လုံး ပေါ်နေသော်လည်း GPU-to-GPU
> bandwidth က မျှော်မှန်းထားသည်ထက် များစွာ နည်းနေသည်။

ကိုယ်ပိုင် plan ရေးပါ — bandwidth ကို ပထမဆုံး ဘယ်လို တိုင်းမလဲ?
service state ကို ဘယ်လို စစ်မလဲ?

### Drill C — Mission 25: Container Runtime — Docker, Enroot & Pyxis with NGC (intermediate)

> Symptom: Slurm အောက်မှ NGC container job များ "CUDA initialization error"
> ဖြင့် fail ဖြစ်နေသည် — host ပေါ်က ပုံမှန် job များကတော့ အလုပ်လုပ်နေသည်။

ကိုယ်ပိုင် plan ရေးပါ — host/container ကို ဘယ်လို ခွဲစစ်မလဲ?
stack အလွှာ (Docker/Enroot/Pyxis/NGC) ကို ဘယ်အစဉ်နဲ့ စစ်မလဲ?

---

plan ၃ ခု ရေးပြီးပါက solution.md ရှိ နမူနာ plan များနှင့် နှိုင်းယှဉ်ပြီး
ကွာခြားချက်များကို miss log တွင် မှတ်ပါ။

## E4. Sandbox Pre-Drill (mission မလုပ်မီ ၅ မိနစ်)

1. Sandbox တွင် **NVLink Down** inject ပြီး `nvidia-smi nvlink -s` output
   ကို မှတ်ပါ
2. **XID Error** inject ပြီး `dmesg | grep -i xid` output ကို မှတ်ပါ
3. **Thermal Issue** inject ပြီး `nvidia-smi` ၏ Perf column ပြောင်းလဲမှု
   ကို မှတ်ပါ
4. Physical action **Reseat NVLink bridge** ကို စမ်းပြီး ဘယ် fault ကို
   ဖြေရှင်းပေးသည် မှတ်ပါ

ဤ symptom များကို mission ထဲ တွေ့ပါက အလိုအလျောက် မှတ်မိစေရန် ဖြစ်သည်။
