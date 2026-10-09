# Module 03 — XID Errors (Troubleshooting Core)

## 1. XID က ဘာလဲ ⭐

GPU driver က kernel ကို အသိပေးသော **error event code** များကို XID ဟု ခေါ်ပါသည် —
`dmesg` (kernel log) ထဲတွင် `NVRM: Xid (PCI:0000:07:00): 79, pid=..., GPU has fallen off the bus` ကဲ့သို့ တွေ့ရပါသည်။

စာမေးပွဲနှင့် လက်တွေ့ ၂ ခုလုံးတွင် **XID ဖတ်တတ်ခြင်း** ကို အမြင့်ဆုံး တန်ဖိုးထားပါသည်။

```bash
dmesg | grep -i xid        # XID event များ ရှာခြင်း ⭐
```

## 2. အရေးကြီး XID ကုဒ် ဇယား ⭐

| XID | အဓိပ္ပာယ် | အမျိုးအစား | ဖြေရှင်းနည်း |
|---|---|---|---|
| **13** | Graphics engine exception | application error | app restart |
| **31** | GPU memory page fault | application/sw | process restart, driver စစ် |
| **43** | GPU stopped responding (hang) | **critical** | GPU reset လိုအပ် |
| **48** | Double-bit ECC error | hardware | GPU replace စဉ်းစား |
| **63** | ECC page retirement | managed | စောင့်ကြည့် |
| **79** | **GPU has fallen off the bus** | **critical HW** | PCIe/reseat/replace |
| **94/95** | Contained/uncontained ECC | HW | ခွဲခြား စစ်ဆေး |
| **119** | GSP timeout | driver/firmware | driver/firmware update |
| **194** | Power brake asserted | power | PSU/power စစ် |

⚠️ ကုဒ် အားလုံး မမှတ်မိရန် မလိုပါ — **43, 79, 13, 31, 48** ကို သေချာ မှတ်ပြီး ကျန်တာကို
"application/HW/driver/power" အမျိုးအစားခွဲ နားလည်ပါ။

## 3. XID 43 vs XID 79 — အဓိက ကွာခြားချက် ⭐

```
XID 43 — GPU stopped responding
  GPU က ရှိနေသေး၊ bus ပေါ် ရှိနေသေး — လုပ်ဆောင်နေရာ ကပ်နေခြင်း
  → nvidia-smi --gpu-reset (software reset ကြိုးစား)

XID 79 — GPU has fallen off the bus
  GPU ကို PCIe bus က မမြင်တော့ — hardware/link ပြဿနာ
  → reseat GPU / PCIe / server power cycle → မကောင်းရင် RMA
```

## 4. SXid — NVSwitch version

NVSwitch မှ ထုတ်သော error ကို **SXid** ဟု ခေါ်ပါသည် — NVLink fabric အတွင်း ပြဿနာ ဖြစ်ပါသည်။
`dmesg | grep -i sxid` ဖြင့် ရှာနိုင်ပါသည်။ XID (GPU) နှင့် SXid (NVSwitch) ကို မရောမိပါစေနှင့်။

## 5. Troubleshooting Workflow ⭐ (D5 အဓိက)

```
① Identify    dmesg | grep -i xid → ကုဒ် ဘာလဲ
② Isolate     ဘယ် GPU / ဘယ် node / ဘယ် component (nvidia-smi, dcgmi)
③ Classify    application? driver? power? hardware?
④ Remediate   reset (software) → reseat (physical) → replace (RMA)
⑤ Validate    nvidia-smi / dcgmi diag → ပြန်ကျန်းမာကြောင်း အတည်ပြု
⑥ Document    nvidia-bug-report.sh (RMA/တိုင်ကြားရန်)
```

## 6. DC Lab Sim ထဲ လေ့ကျင့်ပုံ

Sandbox တွင် **XID Error** inject → `dmesg | grep -i xid` ဖြင့် ဖတ် →
complex scenario ထဲက **GPU Hang (XID 43)** / **Bus Reset (XID 79)** ကို သီးခြား လေ့ကျင့်ပါ —
၂ ခု၏ ဖြေရှင်းနည်း မတူကြောင်း သတိပြုပါ။

## 7. Self-Check

1. "GPU has fallen off the bus" — XID ဘယ်နှစ်လဲ၊ ပထမဆုံး လုပ်သင့်တာ ဘာလဲ။
2. XID နှင့် SXid ကွာခြားချက်?
3. XID 43 အတွက် software reset command?

**အဖြေ** — 1) XID 79 — hardware စစ်/reseat (bus ပေါ် ပြန်တက်လာရန်) · 2) XID = GPU, SXid = NVSwitch · 3) `nvidia-smi --gpu-reset`

## 8. References

- XID errors: https://docs.nvidia.com/deploy/xid-errors/
- NVIDIA GPU RMA process: NVIDIA Enterprise Support
