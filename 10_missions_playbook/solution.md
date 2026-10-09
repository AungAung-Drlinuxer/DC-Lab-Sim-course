# Module 10 — Solution

## E1 နမူနာ (Midnight Deployment)
- hint: "ERR! after shipping" → shipping damage / GPU ချို့ယွင်း
- commands: dmidecode → ipmitool → nvidia-smi → dmesg | grep -i xid → reset → dcgmi diag
- debrief: စက်အသစ်တွင် shipping damage ကို ပထမဆုံး စစ်ဆေးရမည်

## E2
- sequence တစ်ခုချင်း၏ output ကို ရှင်းပြနိုင်ရမည် — ဥပမာ `nvidia-smi` = device state, `dcgmi diag -r 1` = quick health, `sinfo` = job/node state

## E3 နမူနာ plan
- weak: ECC ဖတ်ခြင်း (Module 04 ပြန်) → exercises ပြန်လုပ် → missions ECC-related ပြန်ဖြတ် → quiz ပြန်ဖြေ
