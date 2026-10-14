# Module 18 — အဖြေများ (Worked Answers)

> တစ်ခုချင်းစီအတွက် **ရှာပုံ (lookup path)** ကိုပါ ဖော်ပြထားပါသည် —
> အဖြေ မှန်/မမှန်ထက် ဇယားထဲ ဘယ်လို ရောက်သွားသလဲ ဆိုတာက ဤ drill ၏ ရည်ရွယ်ချက် ဖြစ်ပါသည်။

## Drill 1 — Symptom → XID + Severity + ပထမဆုံး အဆင့်

**ရှာပုံ** — Appendix A တွင် အမည်/အဓိပ္ပာယ် တူညီသော ကြောင်း ရှာပြီး severity group ကြည့်
→ ပထမဆုံး အဆင့်ကို A.4 triage rule မှ ယူပါ။

1. **XID 79 — GPU Fallen Off Bus — Critical**
   ရှာပုံ — "fallen off the bus" စာသားကို A.1 (Critical) တွင် တွေ့ရပါသည်။
   ပထမဆုံး အဆင့် — evidence ရယူ (`dmesg`, `nvidia-bug-report.sh`) ပြီးမှ
   reseat GPU/PCIe → power cycle → မရပါက RMA (A.4)။

2. **XID 76 — ROBUST_CHANNEL_CE7_ERROR (NVLink link training failed) — Critical**
   ရှာပုံ — "link training" စာသားကို A.1 တွင် တွေ့ရပါသည် (XID 74/76/77/78 NVLink group
   နှင့် မရောမိပါစေနှင့် — 76 က training failure ဖြစ်ပါသည်)။
   ပထမဆုံး အဆင့် — Critical ဖြစ်၍ ချက်ချင်း ဆောင်ရွက် — NVLink/bridge/link လမ်းကြောင်း
   စစ်ဆေးခြင်း (Module 06 workflow)၊ SXid log ပါ စစ်ပါ။

3. **XID 43 — GPU Stopped Responding — Warning**
   ရှာပုံ — "stopped responding" ကို A.2 (Warning) တွင် တွေ့ရပါသည်။
   ပထမဆုံး အဆင့် — GPU ပေါ်က process များ ရှင်းပြီး software reset
   (`nvidia-smi --gpu-reset`) — hardware မထိပါနှင့် (A.4)။
   ⚠️ severity က Warning ဖြစ်သော်လည်း job hang ဖြစ်၍ လုပ်ငန်းခွင်တွင် အမြန် ကိုင်တွယ်ရန်
   လိုအပ်ပါသည် (Module 03 §4)။

4. **XID 48 — Double-Bit ECC Error — Critical**
   ရှာပုံ — "double-bit" / "DBE" ကို A.1 တွင် တွေ့ရပါသည် (Appendix B ၏ DBE ဝေါဟာရနှင့်လည်း
   ဆက်စပ်ပါသည်)။
   ပထမဆုံး အဆင့် — job ရပ်/restart (ရလဒ် ယုံ၍ မရ)၊ ECC state မှတ်တမ်းတင်၊
   GPU replace (RMA) စဉ်းစား (A.4)။

5. **XID 38 — Driver Firmware Mismatch — Critical**
   ရှာပုံ — "driver firmware mismatch" ကို A.1 တွင် တွေ့ရပါသည် (Module 04/07 က
   firmware validation နှင့် ဆက်စပ်ပါသည်)။
   ပထမဆုံး အဆင့် — driver/firmware version များ ပြန်စစ်ပြီး ကိုက်ညီအောင် update။

6. **XID 63 — Row Remapping Success — Informational**
   ရှာပုံ — "row remapping success" ကို A.3 (Informational) တွင် တွေ့ရပါသည်
   (XID 64 remapping *failure* — Critical နှင့် မရောမိပါစေနှင့်)။
   ပထမဆုံး အဆင့် — ချက်ချင်း ဘာမှ မလုပ်ပါနှင့် — system က ကိုင်တွယ်ပြီးသား။
   ဖြစ်ပွားမှုနှုန်းကို ဆက်စောင့်ကြည့်ပြီး မှတ်တမ်းတင်ပါ (Module 04 — page retirement/row remap)။

## Drill 2 — ဖော်ပြချက် → Glossary ဝေါဟာရ

**ရှာပုံ** — Appendix B ကို အမျိုးအစား ၅ ခု အလိုက် ကြည့်ပါ — network protocol ဆို B.2,
hardware component ဆို B.3, GPU feature ဆို B.1 စသည်ဖြင့် ခန့်မှန်း ရှာခြင်းက အမြန်ဆုံး ဖြစ်ပါသည်။

1. **SHARP** (B.2) — Scalable Hierarchical Aggregation and Reduction Protocol —
   switch ထဲတွင် reduction လုပ်ပေးသော in-network computing (NCCL နှင့် ပေါင်းသုံး)။
2. **MIG** (B.1) — Multi-Instance GPU — "enabled ≠ usable" trap ကို သတိရပါ (Module 13)။
3. **GDS** (B.2) — GPUDirect Storage။
4. **SM — Subnet Manager** (B.2) — IB subnet စီမံပြီး LID/routes ခွဲဝေပေးသည်။
   ⚠️ အတိုကောက် တူသော **SM — Streaming Multiprocessor** (B.1) က GPU ၏ compute unit ဖြစ်ပါသည် —
   context ပေါ် မူတည်၍ ခွဲခြား ဖတ်ရပါမည်။
5. **PSID** (B.5) — Parameter Set ID — firmware image ပါ board-specific firmware configuration ID
   (NVIDIA MFT documentation အရ Parameter Set ID ဖြစ်ကြောင်း အတည်ပြုပြီး)။
6. **BMC** (B.3) — Baseboard Management Controller — IPMI ဖြင့် remote power/sensor/KVM
   (Module 07 နှင့် ဆက်စပ်)။

## Drill 3 — လုပ်ငန်း → Command Family

**ရှာပုံ** — Appendix C ဇယား၏ "ဤ course မှ အဓိက command များ" တန်းကြီးကို ဖတ်ပြီး
လုပ်ငန်းနှင့် ကိုက်သော family ရွေးပါ — command အမည်ထက် "ဘယ်မေးခွန်းကို ဘယ် tool ဖြေသလဲ"
ကို ကြည့်ပါ (Tool Selection quiz ပုံစံ)။

1. **Monitoring** — `dcgmi dmon -e 1002` (memory utilization field) ဖြင့် real-time
   စောင့်ကြည့်နိုင်ပါသည်။ node တစ်ခုတည်း အတွက် ချက်ချင်း ကြည့်ပါက **GPU Management**
   (`nvidia-smi`) လည်း မှန်ပါသည် — `dcgmi dmon` က streaming/cluster, `nvidia-smi` က
   on-demand/node (Module 02 ဇယား)။
2. **Networking & InfiniBand** — `ibstat` (Module 06) — port state (Active/Down) နှင့် rate
   (HDR 200 / NDR 400) ဖတ်နိုင်ပါသည်။
3. **System Information** — `dmidecode` (Module 07) — SMBIOS/DMI firmware table မှ
   CPU/RAM inventory ဖတ်ခြင်း။
4. **Cluster Management** — `sinfo` (node state/partition, Module 08)၊ job တစ်ခုချင်းအတွက်
   *`squeue`/`sacct`* (Slurm family — simulator တွင် `sinfo` ကို အဓိက သုံးပါသည်)။
5. **NCCL Tests** — `mpirun -np 8 ./all_reduce_perf -b 8 -e 256M -f 2` (Module 15) —
   message size 8B မှ 256MB အထိ all-reduce bandwidth တိုင်းတာခြင်း (ဥပမာ command —
   binary path/process count က site-specific)။
6. **Firmware** — MFT (`mst`)/`mlxup`-style firmware tools (Appendix C, Firmware category) —
   course တွင် procedure အနေဖြင့်သာ ဖော်ပြထားပြီး တိကျသော syntax ကို device ၏
   firmware documentation တွင် အတည်ပြုပါ။

## အကျဉ်းချုပ် — drill များမှ ရရှိသော စည်းမျဉ်းများ

- Symptom တွေ့ပါက **Appendix A → severity → A.4 triage rule** လမ်းကြောင်းအတိုင်း ဖြေပါ —
  ကုဒ် အများကြီး မှတ်မိရန် မလိုပါ (Module 03 က ပြောပြီးသား)။
- ဝေါဟာရ အတိုကောက်တူပါက **context** ကြည့်ပါ (SM သည် ဥပမာ အကောင်းဆုံး)။
- Command ရွေးချယ်ရာတွင် **family-level tool selection** က စာမေးပွဲ ပုံစံဖြစ်ပြီး
  214 commands အားလုံး မှတ်မိရန် မလိုပါ (Appendix C ၏ နှိုင်းယှဉ်ချက်)။
