# Module 04 — Solution

## E1
- ပုံမှန်: counters အားလုံး 0
- ECC inject ပြီး: volatile uncorrectable/correctable counter တက်လာ
- reset ပြီး: volatile = 0 ပြန်၊ aggregate က ကျန် (reset မရ)

## E2 (Severe ECC)
1. identify: `nvidia-smi -q -d ECC` ဖြင့် uncorrectable errors အများအပြား + `dmesg | grep -i xid` (XID 48/94)
2. decide: uncorrectable **ထပ်ခါတလဲလဲ** = hardware ပျက်စီး — GPU ဆက်မသုံးသင့်
3. action: GPU isolate → mark for RMA / replace → `dcgmi diag` ဖြင့် cluster validate
- evidence: ECC counters, XID logs, retired pages, diag result
