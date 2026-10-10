# Module 03 — XID Errors (Troubleshooting Core)

## 1. ပြဿနာ — ဘာကြောင့် XID ဖတ်တတ်ဖို့ လိုအပ်သလဲ

GPU တစ်လုံး ရုတ်တရက် ရပ်သွားခြင်း၊ job ပျက်သွားခြင်း ဖြစ်ပါက application log များကြည့်ရုံနှင့်
root cause ကို မသိနိုင်ပါ — GPU driver က kernel ကို ပေးသော error event code များကသာ
ဘာဖြစ်ခဲ့သလဲ ဆိုတာ ပြောပြပါသည်။ ထို code များကို **XID** ဟု ခေါ်ပြီး ၎င်းတို့ကို ဖတ်တတ်ခြင်းသည်
စာမေးပွဲနှင့် လက်တွေ့ ၂ ခုလုံးတွင် **အမြင့်ဆုံး တန်ဖိုး** ထားခံရသော ကျွမ်းကျင်မှု ဖြစ်ပါသည်။

## 2. XID က ဘာလဲ ⭐

GPU driver က kernel ကို အသိပေးသော **error event code** များကို XID ဟု ခေါ်ပါသည် —
`dmesg` (kernel log) ထဲတွင် `NVRM: Xid (PCI:0000:07:00): 79, pid=..., GPU has fallen off the bus` ကဲ့သို့ တွေ့ရပါသည်။

```bash
dmesg | grep -i xid        # XID event များ ရှာခြင်း ⭐
```

XID log တစ်ကြောင်းတွင် **ကုဒ်နံပါတ်** (ဥပမာ 79), **PCI address** (ဘယ် GPU) နှင့်
**ပတ်သက်သော process/pid** ပါရှိပါသည် — ၃ ခုလုံးကို ဖတ်ပါ။

## 3. အရေးကြီး XID ကုဒ် ဇယား ⭐

| XID | အဓိပ္ပာယ် | အမျိုးအစား | ဖြေရှင်းနည်း |
|---|---|---|---|
| **13** | Graphics engine exception | application error | app restart |
| **31** | GPU memory page fault (FIFO: MMU error) | application/sw | process restart, driver စစ် |
| **43** | GPU stopped responding (hang) | **critical** | GPU reset လိုအပ် |
| **48** | Double-bit ECC error | hardware | GPU replace စဉ်းစား |
| **63** | ECC page retirement | managed | စောင့်ကြည့် |
| **79** | **GPU has fallen off the bus** | **critical HW** | PCIe/reseat/replace |
| **94/95** | Contained/uncontained ECC | HW | ခွဲခြား စစ်ဆေး |
| **119** | GSP timeout | driver/firmware | driver/firmware update |
| **194** | Power brake asserted | power | PSU/power စစ် |

⚠️ ကုဒ် အားလုံး မမှတ်မိရန် မလိုပါ — **43, 79, 13, 31, 48** ကို သေချာ မှတ်ပြီး ကျန်တာကို
"application/HW/driver/power" အမျိုးအစားခွဲ နားလည်ပါ။ XID အများစုက hardware fault
မဟုတ်ပါ — 13, 31, 43 ကဲ့သို့သော ကုဒ်များက application bug (pointer မှားခြင်း၊ out-of-bounds
access ကဲ့သို့) ကြောင့် အများဆုံး ဖြစ်ပြီး 48, 94, 95, 79 ကသာ board/hardware သို့
ညွှန်းပါသည်။

## 4. XID 43 vs XID 79 — အဓိက ကွာခြားချက် ⭐

```
XID 43 — GPU stopped responding
  GPU က ရှိနေသေး၊ bus ပေါ် ရှိနေသေး — လုပ်ဆောင်နေရာ ကပ်နေခြင်း
  → nvidia-smi --gpu-reset (software reset ကြိုးစား)

XID 79 — GPU has fallen off the bus
  GPU ကို PCIe bus က မမြင်တော့ — hardware/link ပြဿနာ
  → reseat GPU / PCIe / server power cycle → မကောင်းရင် RMA
```

ဤ ၂ ခုကို မရောမိပါစေနှင့် — 43 ကို hardware ထိခြင်းဖြင့် စတင်ဖြေရှင်းခြင်းက အချိန်
ဆုံးရှုံးစေပြီး 79 ကို software reset ဖြင့်သာ ဖြေရှင်းခြင်းက ပြဿနာ ဖုံးကွယ်စေပါသည်။

## 5. SXid — NVSwitch version

NVSwitch မှ ထုတ်သော error ကို **SXid** ဟု ခေါ်ပါသည် — NVLink fabric အတွင်း ပြဿနာ ဖြစ်ပါသည်။
`dmesg | grep -i sxid` ဖြင့် ရှာနိုင်ပါသည်။ XID (GPU) နှင့် SXid (NVSwitch) ကို မရောမိပါစေနှင့်။

## 6. Troubleshooting Workflow ⭐ (D5 အဓိက)

```
① Identify    dmesg | grep -i xid → ကုဒ် ဘာလဲ
② Isolate     ဘယ် GPU / ဘယ် node / ဘယ် component (nvidia-smi, dcgmi)
③ Classify    application? driver? power? hardware?
④ Remediate   reset (software) → reseat (physical) → replace (RMA)
⑤ Validate    nvidia-smi / dcgmi diag → ပြန်ကျန်းမာကြောင်း အတည်ပြု
⑥ Document    nvidia-bug-report.sh (RMA/တိုင်ကြားရန်)
```

## 7. လုပ်ငန်းခွင် သတိပြုရန် အချက်များ

- **Reset မလုပ်မီ မှတ်တမ်း အရင်တင်ပါ** — `dmesg` output နှင့် `nvidia-bug-report.sh` ကို
  ရယူပြီးမှ ပြင်ဆင်မှု စပါ။ reset လုပ်ပြီးပါက evidence များ ပျောက်သွားနိုင်ပါသည်။
- **XID အားလုံး hardware ပျက်ခြင်း မဟုတ်ပါ** — application/driver bug ဖြစ်နိုင်သောကြောင့်
  ကုဒ် အမျိုးအစားခွဲ သုံးသပ်ပြီးမှ hardware အဆင့် ဆက်လက် ဆောင်ရွက်ပါ။
- **`nvidia-smi --gpu-reset`** ကို သုံးပါက GPU ပေါ်တွင် process များ ရှိနေပါက မအောင်မြင်နိုင်
  သဖြင့် အရင် process များ ရပ်ရန်နှင့် GPU index (`-i`) ကို သီးခြား ဖော်ပြရန် လိုအပ်ပါသည်
  (ယေဘုယျ အလေ့အကျင့် — tool/driver version ပေါ် မူတည်)။
- XID code ၏ အဓိပ္ပာယ်နှင့် အမျိုးအစားခွဲခြားမှုက driver version အလိုက် အနည်းငယ်
  ပြောင်းလဲနိုင်ပါသည် — တိကျသော အဓိပ္ပာယ်အတွက် လက်ရှိ driver version နှင့် ကိုက်သော
  NVIDIA XID documentation ကို ကိုးကားပါ။

## 8. DC Lab Sim ထဲ လေ့ကျင့်ပုံ

Sandbox တွင် **XID Error** inject → `dmesg | grep -i xid` ဖြင့် ဖတ် →
complex scenario ထဲက **GPU Hang (XID 43)** / **Bus Reset (XID 79)** ကို သီးခြား လေ့ကျင့်ပါ —
၂ ခု၏ ဖြေရှင်းနည်း မတူကြောင်း သတိပြုပါ။

## 9. Self-Check

1. "GPU has fallen off the bus" — XID ဘယ်နှစ်လဲ၊ ပထမဆုံး လုပ်သင့်တာ ဘာလဲ။
2. XID နှင့် SXid ကွာခြားချက်?
3. XID 43 အတွက် software reset command?
4. XID 63 က ဘာကို ဆိုလိုသလဲ၊ ဘာလုပ်သနည်း။
5. Troubleshooting workflow ၏ အဆင့် ၆ ဆင့်ကို အမည်တပ်ပါ။

**အဖြေ** — 1) XID 79 — hardware စစ်/reseat (bus ပေါ် ပြန်တက်လာရန်) · 2) XID = GPU, SXid = NVSwitch · 3) `nvidia-smi --gpu-reset` · 4) ECC page retirement (managed) — ဖြစ်ပွားမှုနှုန်းကို ဆက်စောင့်ကြည့် · 5) Identify → Isolate → Classify → Remediate → Validate → Document

## 10. References

- XID errors: https://docs.nvidia.com/deploy/xid-errors/
- NVIDIA GPU RMA process: NVIDIA Enterprise Support

## Technical review notes

- XID 43 ၏ NVIDIA ရုံးတရားဝင် XID catalog အမည်မှာ "Reset Channel Verif Error / GPU stopped
  processing" ဖြစ်ပြီး လက်တွေ့တွင် "GPU stopped responding (hang)" ဟု အသုံးများသည် —
  အဓိပ္ပာယ် အတူတူပင် ဖြစ်ပြီး မူရင်းစာသားကို ထိန်းသိမ်းထားသည်။
- XID 194 "Power brake asserted" ကို လက်ရှိ NVIDIA XID catalog (r555) တွင် အတည်မပြုနိုင်သေးပါ —
  **needs verification**။
- XID 94 (contained) နှင့် 95 (uncontained) သည် ECC uncorrectable error ၂ မျိုးခွဲခြားခြင်း ဖြစ်ပြီး
  95 (uncontained) က GPU reset/replace လိုအပ်နိုင်သည် — မူရင်း "ခွဲခြား စစ်ဆေး" ဆိုခြင်းကို
  ထိန်းသိမ်းထားသည် (**needs verification** — remediation အဆင့်က driver/platform ပေါ် မူတည်)။
- XID 48 (DBE) ၏ ဖြေရှင်းနည်းအဖြစ် "GPU replace စဉ်းစား" ကို ထိန်းသိမ်းထားသည် — XID 64
  (page retirement/row remap recording failure) လည်း RMA အဆင့် ဖြစ်ကြောင်း NVIDIA
  documentation တွင် တွေ့ရသည် (ဇယားတွင် ထည့်သွင်းခြင်း မပြုပါ)။
