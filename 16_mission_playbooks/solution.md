# Module 16 — Solution: နမူနာ Triage Plans

> ဤနေရာရှိ plan များသည် နမူနာ တစ်ခုသာ ဖြစ်သည် — ကိုယ်ပိုင် plan တွင်
> အဆင့်စဉ် ကွဲပြားသော်လည်း fault class ခွဲခြားမှုနှင့် အတည်ပြုမှု ပါပါက
> မှန်ကန်သော plan ဖြစ်သည်။

## Drill A — Mission 16: The NVLink Mystery (နမူနာ plan)

**ဖြစ်နိုင်ချေ fault classes (priority အလိုက်)**

1. NVLink link loss / degradation (XID 74/76/77 ပါလာနိုင်)
2. Fabric Manager service ပြဿနာ
3. NVLink bridge physical seating

**Command အဆင့်စဉ်**

1. `nvidia-smi nvlink -s` — link အလိုကး state စစ် — Inactive/0 MB/s ရှာ
2. `nvidia-smi nvlink -e` — error counter ဖြင့် ဘယ် link ပျက်နေသည် ခန့်မှန်း
3. `dmesg | grep -i xid` — NVLink XID 74/76/77/78 ရှိ/မရှိ
4. `nvidia-smi topo -m` — topology ပုံမှန်/မပုံမှန် နှိုင်းယှဉ်

**Remediation အဆင့်စဉ်**

`systemctl restart nvidia-fabricmanager` → မရပါက physical
**Reseat NVLink bridge** → ထပ်ဖြစ်ပါက **Mark for RMA**

**အတည်ပြုခြင်း** — `nvidia-smi nvlink -s` တွင် link အားလုံး Active +
single-node all-reduce test ဖြင့် 8-GPU scaling ပြန်ရမှ ပြီး

**နှိုင်းယှဉ်စရာ** — playbook နှင့် ကွာပါက "performance ကျခြင်း = link
state အရင် စစ်" ဆိုသော အခြေခံ မှတ်မိ/မမှတ်မိ ကို ပြန်စစ်ပါ။

## Drill B — Mission 12: NVLink & Fabric Manager Activation (နမူနာ plan)

**ဖြစ်နိုင်ချေ fault classes**

1. Fabric Manager မ start ရသေးခြင်း (mission brief နှင့် အကိုက်ဆုံး)
2. NVSwitch fabric initialize မပြီးခြင်း
3. NVLink link အချို့ Inactive

**Command အဆင့်စဉ်**

1. bandwidth ကို **state ဖြင့် သွယ်ဝိုက် တိုင်း** — `nvidia-smi nvlink -s`
   တွင် link အားလုံး Active + full rate ဖြစ်/မဖြစ်
2. `nvidia-smi topo -m` — GPU ၈ လုံးကြား NVLink connection များ မြင်ရမှု
3. Fabric Manager service အခြေအနေ စစ် — မလုပ်ရသေးပါက
   `systemctl restart nvidia-fabricmanager`
4. `nvidia-smi nvlink -s` ပြန်စစ် — link အားလုံး Active
5. single-node all-reduce test ဖြင့် bandwidth အတည်ပြု

**Remediation အဆင့်စဉ်** — service restart က ပထမ (config/software ဘက်) —
မရပါကမှ physical **Reseat NVLink bridge**

**အတည်ပြုခြင်း** — `nvlink -s` အားလုံး Active + `dcgmi diag -r 1` pass

**နှိုင်းယှဉ်စရာ** — "GPU ပေါ်နေတာနဲ့ ကောင်းတာ" ဟု မယူမိဘဲ link
state အထိ ဆင်းစစ်မိပါက plan မှန်သည်။

## Drill C — Mission 25: Container Runtime — Docker, Enroot & Pyxis with NGC (နမူနာ plan)

**ဖြစ်နိုင်ချေ fault classes**

1. Container အတွင်း GPU pass-through ပျက်ခြင်း (toolkit config)
2. Driver/toolkit version mismatch
3. Pyxis/Enroot integration ပြဿနာ (Slurm အောက်မှသာ)

**Command အဆင့်စဉ် — host/container ခွဲခြား**

1. host ပေါ် `nvidia-smi` — driver ကောင်း/မကောင်း (host ပျက်နေပါက
   container ဘက် မဟုတ် — ဤနေရာတွင် host ပုံမှန်ဟု ဆိုထားသဖြင့် ဆက်)
2. `docker run --gpus all nvidia/cuda:12.x nvidia-smi` — local container
   ထဲ GPU မြင်/မမြင် ⭐ (ဤအဆင့် အောင်ပါက toolkit ကောင်း —
   ပြဿနာက Slurm/Pyxis ဘက် ရွှေ့)
3. `sbatch --container-image=nvidia/cuda:12.x job.sh` — Pyxis/Enroot
   integration ဖြင့် job တစ်ခု တင်ကြည့်
4. fail ဆက်ပါက job log ထဲ error ကို ဖတ်ပြီး stack အလွှာ ခွဲ —
   Pyxis (Slurm plugin) vs Enroot (runtime) vs NGC (image source)
5. `ngc --version` — image ဆွဲယူမှု ပြဿနာ ပါ/မပါ ဖယ်ရှား

**Remediation အဆင့်စဉ်** — toolkit config ပြင်/restart → version
ကိုက်ညီအောင် ပြင် → Pyxis/Enroot config ပြင် (install → configure →
restart → verify)

**အတည်ပြုခြင်း** — container job အောင်မြင်ပြီး container ထဲ
`nvidia-smi` GPU မြင်ကြောင်း log ဖြင့် သက်သေပြ

**နှိုင်းယှဉ်စရာ** — Docker ဖြင့် local verify အောင်ပြီး Slurm အောက်မှ
သာ fail ခြင်းကို ခွဲမိပါက plan မှန်သည် — stack အလွှာ တစ်ခုချင်း၏
အခန်းကဏ္ဍ (Docker = local verify, Enroot = runtime, Pyxis = Slurm
integration, NGC = image source) ကို မှတ်မိပါက ပြီးပြီ။

---

## E1/E2/E4 workflow အတွက် အဖြေ အကျဉ်း

- **E1** — log ဇယားကို mission တစ်ခုချင်း ဖြည့်ပြီး "လွဲခဲ့သော အဆင့်"
  ကောင်းကောင်း မှတ်ပါ — မအောင်ခြင်းထက် **ဘယ်လို လွဲသည်** က
  ပိုအရေးကြီးသည်။
- **E2** — miss များကို fault class အလိုက် ပြန်လည် အုပ်စုခွဲပြီး ဆက်စပ်
  module ဖြင့် ပြန်ဖတ် — တစ်ပတ် အကြာတွင် ထို mission များ ပြန်လုပ်ပါ။
- **E4** — sandbox symptom များကို ကြိုမှတ်ထားပါက mission ထဲ တွေ့သော
  အခါ identify ချိန် သိသိသာသာ တိုသွားပါမည်။

## Technical review notes

- နမူနာ plan များသည် course command families (nvidia-smi, dcgmi, dmesg,
  ibstat, systemctl, Docker/Pyxis/NGC) ကိုသာ အသုံးပြုထားသည် —
  simulator က command တစ်ခုချင်းကို လက်ခံ/မလက်ခံ ဆိုသည်ကို simulator
  terminal တွင် အတည်ပြုပါ (needs verification)။
- mission ၃ ခု၏ အတိအကျ injected faults ကို မသိရသဖြင့် နမူနာ plan များကို
  brief ပေါ် အခြေခံ၍ ရေးထားခြင်း ဖြစ်သည် — fault အမျိုးအစား မတူပါက
  ချဉ်းကပ်မှု အဆင့်စဉ်သည် သက်ရောက်မှု ရှိနိုင်သည်။
