# Module 03 — Exercise

## E1. XID ဖတ်လေ့ကျင့်ခြင်း
1. Sandbox တွင် GPU 1 ၌ **XID Error** inject ပါ
2. `dmesg | grep -i xid` run ပြီး XID ကုဒ် နှင့် message အပြည့် မှတ်တမ်းတင်ပါ
3. `nvidia-smi` ဖြင့် GPU 1 ၏ state ကို ကြည့်ပါ — error ပြနေလား

## E2. Complex Scenario — GPU Hang
1. **Complex Training Scenarios → GPU Hang (XID 43)** ကို run ပါ
2. `dmesg | grep -i xid` ဖြင့် XID 43 အတည်ပြုပါ
3. ဖြေရှင်းရန် remediation command စဉ်းစားပြီး —
   `nvidia-smi --gpu-reset -i 0` ကို စမ်းသပ်ပါ
4. `nvidia-smi` ဖြင့် ပြန်ကျန်းမာကြောင်း အတည်ပြုပါ

## E3. Complex Scenario — Bus Reset
1. **Bus Reset (XID 79)** scenario run ပါ
2. XID 79 message ကို ဖတ်ပြီး — GPU hang နှင့် ဘာကွာသနည်း ရှင်းပြပါ
3. Physical Actions ထဲက **Reseat GPU** ကို အသုံးပြုပြီး ရလဒ် ကြည့်ပါ

## E4. Bug report
`nvidia-bug-report.sh` က ဘာအတွက် သုံးသနည်း — တစ်ကြောင်း ရေးပါ
