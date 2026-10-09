# Module 03 — Solution

## E1
- dmesg တွင် `NVRM: Xid ...` ကြောင်း တွေ့ရမည် — ကုဒ်နှင့် အသေးစိတ် ပါရမည်
- `nvidia-smi` တွင် GPU 1 error state (သို့) message အနီရောင် ပြနိုင်

## E2 (XID 43)
- XID 43 = GPU stopped responding — software reset ကြိုးစားရမည့် အခြေအနေ
- `nvidia-smi --gpu-reset -i 0` → reset အောင်မြင်ပါက GPU ပြန်ကျန်းမာ
- မအောင်မြင်ပါက physical (reseat) → node restart အဆင့် ဆက်

## E3 (XID 79)
- XID 79 = "GPU has fallen off the bus" — PCIe/hardware ပြဿနာ (GPU ကို bus က မမြင်)
- GPU hang ထက် ပြင်းထန် — software reset မလုံလောက်
- **Reseat GPU** → power cycle → မကောင်းလျှင် **Mark for RMA**

## E4
- `nvidia-bug-report.sh` = diagnostic log package စုဆောင်း — NVIDIA support / RMA တိုင်ကြားရာတွင် ပူးတွဲ တင်ပြရန်
