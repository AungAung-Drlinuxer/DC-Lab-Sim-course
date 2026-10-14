# Module 17 — Solution (Worked Samples)

## E1 — Schedule ဖြင့် လေ့ကျင့်ခြင်း နမူနာ ရလဒ်

Day 5 Full Practice တွင် နမူနာ မှတ်တမ်း ပုံစံ —

```
Day 5: Full Practice 60Q / 90 မိနစ် → 43/60 (71.7%)
  D1 Bring-Up     ခန့်မှန်း 19 ခု မှ 13 ခု မှန်
  D4 Test/Verify  ခန့်မှန်း 20 ခု မှ 12 ခု မှန်  ← အားနည်း
  D3/D5/D2        ကျန် 21 ခု မှ 18 ခု မှန်
```

ဆုံးဖြတ်ချက် — 70%+ ရသော်လည်း margin ပါးပါး ဖြစ်သောကြောင့် —

- Day 6 Review Mistakes တွင် D4 မှားသည့် ၈ ခု ကို ဦးစားပေး ပြန်ဖြေပါ
- Day 7 မတိုင်မီ Module 15 (test & verification) ပြန်ဖတ်ပါ
- Day 10 ဒုတိယ Full Practice တွင် D4 တိုးတက်မှု ပြန်တိုင်းပါ
- 85%+ margin ရအောင် (Module 11 အကြံပြုချက်) မရမချင်း စက်ဝန်း ပြန်စပါ

Day 8 Quick Quiz ကို Day 1 နှင့် နှိုင်းယှဉ်ပါ — 10/15 → 13/15 ဆိုပါက
အားနည်းချက် ပြန်ဖတ်မှု အလုပ်ဖြစ်ကြောင်း သိရပါသည် (Quick Quiz က 1 မိနစ်/ခု
ဖြစ်သောကြောင့် အမြန်နှုန်းလည်း တိုးတက်မှု မှတ်ပါ)။

## E2 — Drill 1 နမူနာ (ကိုယ်တိုင် ရေးသော 15-Question Quiz မှ ၅ ခု)

ဤ မေးခွန်း များသည် ဤ course အကြောင်းအရာ (Module 01–08) ပေါ် ကိုယ်တိုင်
ဖန်တီးထားသော နမူနာ ဖြစ်ပြီး simulator ၏ မေးခွန်းဘဏ် မဟုတ်ပါ။

**Q1 (multiple choice)** — `dcgmi diag` ဖြင့် acceptance test အတွက် အနက်ရှိုင်းဆုံး
diagnostic လုပ်ရန် ဘယ် command ကို သုံးမလဲ?

- A) `dcgmi diag -r 1`
- B) `dcgmi diag -r 2`
- C) `dcgmi diag -r 3` ✅
- D) `dcgmi health -s`

**အကြောင်းပြချက်** — `-r 1` က quick၊ `-r 2` က standard၊ `-r 3` က deep —
acceptance/certification အတွက် deep လိုအပ်ပါသည် (Module 02, 15)။
`health -s` က health watch အခြေအနေ ကြည့်ရုံ ဖြစ်ပါသည်။

**Q2 (tool output ဖတ်ရှုခြင်း)** — `ibstat` output တွင် port တစ်ခုက
`State: Active`၊ `Rate: 100 Gb/s` ပြနေပါသည် (DGX A100 node)။ ဘာ သံသယ ရှိသင့်သလဲ?

**အဖြေ** — link တက်နေသော်လည်း DGX A100 တွင် မျှော်မှန်းထားသည်က **HDR 200Gb/s**
ဖြစ်ပါသည် — 100 Gb/s ဆိုပါက full speed မရောက်သော link (cable/transceiver/signal
quality) ကို သံသယ ရှိရပါမည် — "State Active ≠ Rate မှန်" (Module 06)။

**Q3 (sequence)** — thermal alert ရရှိပြီး `nvidia-smi -q -d TEMPERATURE` တွင်
GPU အပူချိန် မြင့်နေကြောင်း အတည်ပြုပြီးပါပြီ။ နောက် အဆင့် အကောင်းဆုံး က ဘာလဲ?

- A) `dcgmi diag -r 3` ချက်ချင်း run
- B) `ipmitool sensor list` ဖြင့် chassis/cooling level (inlet temp, fan) စစ်ဆေး ✅
- C) GPU ကို ချက်ချင်း ပြန်လည် စတင်
- D) driver update လုပ်

**အကြောင်းပြချက်** — GPU အပူချိန် မြင့်ခြင်းက system cooling level နှင့်
ဆက်စပ်ပါသည် — `ipmitool sensor list` ဖြင့် inlet temp/fan RPM ကို စစ်ပြီးမှ
root cause သိရပါမည် (Module 05)။ deep diag က အချိန်ကြာပြီး load ပိုပေးနိုင်ပါသည်။

**Q4 (multiple choice)** — Slurm node တစ်ခုက `drained` အခြေအနေ ဆိုပါက — ဘာ ဆိုလိုသလဲ?

- A) node က job မရှိဘဲ idle
- B) node က ပျက်နေ (down)
- C) node က admin ဖြင့် ရည်ရွယ်ချက် ရှိရှိ ဖယ်ရှားထားခြင်း (maintenance) ✅
- D) node က job ပြည့်နေ

**အကြောင်းပြချက်** — idle ≠ down ≠ drained — drained က maintenance/ထိန်းသိမ်းမှု
အတွက် ရည်ရွယ်ချက် ရှိရှိ ဖယ်ရှားထားခြင်း ဖြစ်ပါသည် (Module 08)။

**Q5 (tool output ဖတ်ရှုခြင်း)** — `dmesg | grep -i xid` တွင် "XID 79" တွေ့ပါသည် —
severity နှင့် ပထမ ဆုံး စဉ်းစားရမည့် action က ဘာလဲ?

**အဖြေ** — XID 79 = GPU fallen off bus — **Critical** (hardware/PCIe level)။
software reset တစ်ခုတည်းနှင့် မလုံလောက် — reseat/power cycle → မရပါက
Mark for RMA အထိ စဉ်းစားရပါမည် (Module 03, 09 — site XID reference)။

## E3 — Drill 2 နမူနာ (Pacing Analysis)

```
Day 5 Full Practice pacing မှတ်တမ်း —
  1.5 မိနစ် (90s) အောက်: 48 မေးခွန်း
  90s ကျော်:            12 မေးခွန်း  ← ပြန်စစ်ရန်
    မှ ၉ ခု = tool output ဖတ်ရှုမှု မေးခွန်း (output အရှည် ဖတ်ရသောကြောင့်)
    မှ ၃ ခု = sequence မေးခွန်း (ရွေးချယ်စရာ ၄ ခု အားလုံး ပြန်ဖတ်ရသောကြောင့်)
```

ပြင်ဆင်ချက် —

1. tool output မေးခွန်း တွင် output ကို အလယ်မှ စ မဖတ်ဘဲ header/field name →
   ပြဿနာ ရှိနိုင်သော line ဟု ယူဆရသော အပိုင်း ကို ဦးစားပေး ဖတ်ပါ
2. sequence မေးခွန်း တွင် "NEXT / FIRST / BEST" စကားလုံး ကို အရင် ရှာပါ
   (Module 11 §5 trap) — ဖတ်ရှုမှု မြန်ခြင်းကြောင့် မှားခြင်း မဖြစ်စေပါနှင့်
3. 90s ထက် ကျော်သော မေးခွန်း ကို ချက်ချင်း flag လုပ်ပြီး နောက်မှ ပြန် —
   တစ်ခုတည်းတွင် အချိန် ၃–၄ မိနစ် မကုန်ပါစေနှင့်

## E4 — Drill 3 နမူနာ (Sandbox Triage Log)

```
Fault: NVLink Down (Sandbox injection)
  ကြိမ် 1: diagnosis 2:40 · fix 3:10
    State  → nvidia-smi: GPU အားလုံး မြင်ရ (GPU down မဟုတ်)
    Logs   → dmesg: XID 74/76 အမျိုးအစား NVLink error တွေ့
    Fabric → nvidia-smi nvlink -s: link အချို့ down
    Physical → ipmitool sensor list: ပုံမှန် (chassis ပြဿနာ မဟုတ်)
    Fix    → Reseat NVLink bridge → validate (nvlink -s ပြန်စစ်)
  ကြိမ် 2: diagnosis 1:35 · fix 2:20  ← 39% မြန်လာ
  ကြိမ် 3: diagnosis 1:10 · fix 2:05
```

ထို ကျွမ်းကျင်မှု ဖြင့် Live Incidents အမှတ်ပေး စနစ်ထဲ ဝင်ပါက —
diagnosis အချိန် တိုတောင်းခြင်းက rating အတွက် အကျိုးပြုပါမည်
(rating စည်းမျဉ်း အသေးစိတ်က site တွင် မဖော်ပြ — needs verification)။

## E5 — စာမေးပွဲ မတိုင်မီ checklist နမူနာ

- [x] Full Practice ၂ ကြိမ် — Day 5: 71.7% · Day 10: 78.3% (85%+ မရသေး —
      Day 11–12 ပြန်ဖတ်ပြီး ထပ်ဖြေ)
- [x] Tool Mastery 7 families အားလုံး ဖြေပြီး — အနိမ့်ဆုံး XID Diagnostics (26/35)
      → Module 03 + XID reference ပြန်ဖတ်ပြီး ပြန်ဖြေ
- [x] Review Mistakes ထဲက မေးခွန်း အားလုံး ရှင်းပြနိုင်ပြီ
- [x] Sandbox triage drill ၃ ကြိမ် ပြီး (diagnosis အချိန် တိုးတက်)

📌 70%+ ဆိုသည်မှာ gate ဖြစ်ပြီး 78% ခန့် ဖြင့် မဖြေသင့်သေးပါ —
Module 11 အတိုင်း 85%+ ပုံမှန် ရအောင် ဆက်လေ့ကျင့်ပြီးမှ စာရင်းသွင်းပါ။

## Technical review notes

- ဤ solution ရှိ မေးခွန်း နမူနာ များနှင့် ရလဒ် ဂဏန်းများ (71.7% စသည်) သည်
  လေ့ကျင့်မှု ပုံစံ ပြရန် ဖန်တီးထားသော နမူနာ ဖြစ်ပြီး simulator ၏ မေးခွန်းဘဏ်
  သို့မဟုတ် စစ်တမ်း ရလဒ် မဟုတ်ပါ။
- E2 ၏ အဖြေ များသည် ဤ course Module 02/03/05/06/08/15 နှင့် site ၏
  XID Error Code Reference ပေါ် အခြေခံပြီး ရေးထားခြင်း ဖြစ်သည် —
  simulator တွင် ပြသသော အတိအကျ output format က ကွဲနိုင်ပါသည် (needs verification)။
- E4 ရှိ rating နှင့် ပတ်သက်သော မှတ်ချက်သည် conceptual ဖြစ်ပြီး site က
  rating ပြောင်းလဲမှု စည်းမျဉ်း အသေးစိတ် မဖော်ပြပါ (needs verification)။
