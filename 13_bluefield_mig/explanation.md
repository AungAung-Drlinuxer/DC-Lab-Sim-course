# Module 13 (Knowledge) — BlueField DPU & MIG (D2 — 5%)

> D2 က weight ငယ်ပေမယ့် BlueField + MIG က ရှုပ်ထွေးပြီး ရမှတ် လွယ် —
> ဤ module တွင် concept + configuration + tools usage အပြည့် ရှင်းပြပါမည်။

---

## 1. ပြဿနာ — infrastructure load နှင့် GPU sharing ပြဿနာ

server တစ်ခုတွင် network/storage/security stack အားလုံးကို CPU က ကိုင်ရပါက workload အတွက်
CPU core များ လျော့သွားပါသည်။ တစ်ဖက်တွင် GPU တစ်ခုလုံးကို user/workload တစ်ခုတည်း ပေးလိုက်ပါက
အသုံးပြုနိုင်စွမ်း ကျန်ပါသည် — မျှဝေ သုံးပါကလည်း workload ချင်း interference နှင့် isolation
ပြဿနာ ရှိပါသည်။ ဤပြဿနာ ၂ ခုကို အသီးသီး **BlueField DPU** (infrastructure offload) နှင့်
**MIG** (hardware-level partition) က ဖြေရှင်းပါသည်။

## 2. Core Concept — BlueField DPU က ဘာလဲ ⭐

**DPU (Data Processing Unit)** = networking/storage/security ကို CPU မှ ဖယ်ရှား
ကိုယ်ပိုင် processor ဖြင့် လုပ်ဆောင်ပေးသော SmartNIC — NVIDIA BlueField မိသားစု။

```
┌─────────────────────────────────────────────┐
│  BlueField DPU                              │
│  ┌─────────┐  ┌──────────┐  ┌───────────┐   │
│  │ARM cores│  │ConnectX  │  │ crypto/   │   │
│  │(DOCA)   │  │NIC (RDMA)│  │ compress  │   │
│  └─────────┘  └──────────┘  └───────────┘   │
└─────────────────────────────────────────────┘
   ↳ infrastructure offload: network, storage, security
```

- **Offload** — NIC/switch/storage function ကို DPU က ကိုင် → CPU က workload အတွက် လွတ်
- **Isolation** — infrastructure software က user workload နှင့် သီးခြား (security)
- **DOCA** = DPU အတွက် platform SDK (CUDA က GPU အတွက် ကဲ့သို့) ⭐

**ဘယ်အခါ သုံးမလဲ** — multi-tenant data center တွင် infrastructure function ကို host CPU နှင့်
သီးခြား ခွဲထုတ်လိုပါက BlueField သုံးပါသည်။ **အားနည်းချက်** — DOCA/BFB software stack
ထပ်စီမံရပြီး ကုန်ကျစရိတ် ထပ်ဝင်ပါသည်။

## 3. BlueField Modes of Operation ⭐⭐ (စာမေးပွဲ အမေးများ)

| Mode | ဘယ်လို အလုပ်လုပ် | ဘယ်အခါ |
|---|---|---|
| **Embedded/DPU mode (ECPF)** | ARM cores က NIC resource နှင့် data path ကိုင် (default) | ပြည့်စုံ offload |
| **Restricted mode** | Embedded mode + host က DPU ကို ဝင်ခွင့် ကန့်သတ် | security အလွန် လိုအပ်သော နေရာ |
| **NIC mode** | DPU က ပြင်ပ host အတွက် ConnectX NIC ကဲ့သို့သာ ပေါ်လာ | offload မလို |
| **Separated host mode** | DPU/host ကို symmetric model ဖြင့် သီးခြား စီမံ | DPU ကို host နှင့် သီးခြား စီမံလိုသောအခါ |

configuration: DOCA ဖြင့် (`doca` command/API) — DOCA install က D3 ၏
"GPU နှင့် DOCA drivers" topic နှင့် ဆက်စပ်ပါသည်။ mode အမည်များကို DOCA "BlueField Modes of
Operation" documentation အတိုင်း ရည်ညွှန်းထားပါသည်။

## 4. Core Concept — MIG (Multi-Instance GPU) ⭐⭐

GPU ကို **hardware-level partition** ခွဲခြင်း — SM/memory/cache သီးခြား။

![without MIG](../images/mig-multiuser-before.png)
*multi-user sharing (isolation မရှိ)*

![with MIG](../images/mig-multiuser-after.png)
*MIG instance များ (hardware-isolated)*

- Ampere generation မှ စ (compute capability ≥ 8.0) data center GPU များတွင် ရပါသည်
- instance **အများဆုံး အရေအတွက်** က GPU model ပေါ် မူတည် — A100/H100/H200 တွင် **7 ခု** အထိ
  (A30 ကဲ့သို့ GPU များတွင် 4 ခုသာ) — စာမေးပွဲ အဖြေအတွက် "7 instances" ကို မှတ်ပါ
- QoS တည်ငြိမ် — inference workload မျှဝေအတွက် အကောင်းဆုံး

### MIG Profile (H100 80GB ဥပမာ)

| Profile | Memory | instances |
|---|---|---|
| 1g.10gb | 10GB | 7 |
| 1g.20gb | 20GB | 4 |
| 2g.20gb | 20GB | 3 |
| 3g.40gb | 40GB | 2 |
| 4g.40gb | 40GB | 1 |
| 7g.80gb | 80GB | 1 |

profile နှင့် instance အရေအတွက် က GPU generation/model ပေါ် မူတည် — အထက်ပါ ဇယားက H100 80GB
အတွက် ဥပမာ ဖြစ်ပြီး အခြား GPU တွင် profile အမည်/အရေအတွက် ကွဲနိုင်ပါသည်။

## 5. Practical Configuration — MIG Tools Usage ⭐

```bash
sudo nvidia-smi -mig 1                        # MIG mode ဖွင့် (GPU ပေါ် အလုပ်မရှိမှ ပြောင်းနိုင်)
nvidia-smi mig -lgip                           # GPU instance profile များ (ID/အမည်) စစ်ရန်
nvidia-smi mig -lcip                           # compute instance profile များ စစ်ရန်
sudo nvidia-smi mig -cgi 3g.40gb,3g.40gb -C    # profile အမည်ဖြင့် instance ၂ ခု ဖန်တီးပြီး attach
sudo nvidia-smi mig -lgi                       # ဖန်တီးပြီး GPU instance များ စာရင်း
sudo nvidia-smi mig -dgi -gi 0                 # GI ID 0 ကို ဖျက်ရန်
```

- `nvidia-smi -mig 1` — GPU ကို MIG mode ပြောင်းရုံ — ဤအဆင့်တွင် instance မရှိသေး
- `mig -lgip / -lcip` — ရနိုင်သော profile (memory/SM fraction)၊ ID နှင့် instance အများဆုံး အရေအတွက် ကြည့်ရန်
- `mig -cgi ... -C` — profile အမည် သို့ ID ဖြင့် instance ဖန်တီး (`-C` = ဖန်တီးပြီး ချက်ချင်း configure)
- `mig -lgi` — ဖန်တီးပြီး instance များ စာရင်းကြည့် / `mig -dgi` — instance ဖျက်

⚠️ **"enabled ≠ usable" trap** — `nvidia-smi -mig 1` က enable သာ —
instance ဖန်တီးမှ workload ထည့်နိုင် — စာမေးပွဲက ဒုတိယ အဆင့်ကို သက်သေပြ command မေးလေ့ရှိ။ ⭐

## 6. လုပ်ငန်းသုံး သတိပြုရန် အချက်များ

- MIG mode ပြောင်းခြင်းသည် GPU reset လိုအပ်နိုင် — GPU ပေါ် အခြား workload ရှိပါက ရပ်တန့်နိုင်
  (GPU model/driver ပေါ် မူတည်)
- profile ပေါင်းစပ်မှုသည် GPU ၏ memory/SM fraction အတွင်းသာ ဖြစ်ရ — ကျော်လွန်ပါက instance ဖန်တီး၍ မရ
- GPU instance (GI) ID နှင့် profile ID ကို ရှုပ်ပါက ဖျက်မိ/ဖန်တီးမိ နိုင် — `mig -lgi` ဖြင့် အမြဲ အတည်ပြုပါ
- DOCA/BFB version သည် BlueField generation နှင့် ကိုက်ညီရ — version matrix စစ်ပါ

## 7. Self-Check

1. DPU က CPU ကနေ ဘာတွေ ဖယ်ရှားပေးသလဲ?
2. MIG enable လုပ်ပြီး workload ထည့်နိုင်ပြီလား?
3. DOCA က ဘာအတွက် platform?
4. BlueField ၏ mode ၄ မျိုး အမည် ပြောပြနိုင်သလား?

**အဖြေ** — 1) network/storage/security · 2) မရသေး — instance ဖန်တီးမှ (enabled ≠ usable) ·
3) DPU · 4) Embedded/DPU mode (ECPF), Restricted mode, NIC mode, Separated host mode

## 8. အခြား sources ရှင်းလင်းချက်

- **NVIDIA MIG User Guide** — "MIG mode" နှင့် "instance" ကို concept ၂ ခုအဖြစ် ခွဲ ဖော်ပြ
- **aguidetocloud.com** — "enabled vs usable" trap ကို MIG ၏ သီးခြား ဥပမာအဖြစ် အလေးပေး
- **Ravikiran guide** — BlueField topic ကို DOCA docs နှင့် ချိတ် — mode of operation
  ကို "platform management" အဖြစ် မြင်ရန် အကြံပြု

## 9. References & Further Reading

- BlueField/DOCA: https://docs.nvidia.com/networking/dpu-doca/
- DOCA Install: https://docs.nvidia.com/doca/archive/2-9-2/doca+installation+guide+for+linux/
- MIG User Guide: https://docs.nvidia.com/datacenter/tesla/mig-user-guide/
- MIG Profiles: https://docs.nvidia.com/datacenter/tesla/mig-user-guide/supported-mig-profiles.html

## Technical review notes

- ပြင်ဆင်မှု — MIG profile ဇယားကို NVIDIA MIG User Guide (H100 80GB) အတိုင်း ပြင်ဆင် —
  မူလ "1g.10gb = 8 (သို့) 7" နှင့် "2g.20gb = 4" သည် မမှန်ကန် (1g.10gb = 7, 2g.20gb = 3)။ 1g.20gb/4g.40gb များ ထည့်သွင်းခဲ့သည်။
- ပြင်ဆင်မှု — "instance အမြင့်ဆုံး 7 ခု" ကို A100/H100/H200 class အတွက်ဟု သတ်မှတ် — A30 တွင် 4 ခုသာ (MIG User Guide, Supported GPUs)။
- ပြင်ဆင်မှု — profile စစ် command ကို `nvidia-smi -lgc/-lmc` (clock limit စစ်သော command) မှ
  `nvidia-smi mig -lgip/-lcip` သို့ ပြောင်းခဲ့သည်။
- ပြင်ဆင်မှု — `mig -cgi` ဥပမာကို numeric ID (`9,9`) မှ profile အမည် (`3g.40gb`) ဖြင့် ပြောင်း —
  numeric ID များသည် GPU model ပေါ် မူတည်သဖြင့် `mig -lgip` output ကြည့် သတ်မှတ်ရပါမည်။
- ပြင်ဆင်မှု — BlueField mode စာရင်းတွင် Restricted mode ထည့်သွင်း — mode အမည်များကို DOCA
  "BlueField Modes of Operation" ဖြင့် အတည်ပြုပြီး။
- MIG mode enable/reset အပြုအမူ (restart/reboot လို/မလို) သည် GPU model နှင့် driver version ပေါ် မူတည် (needs verification)။
