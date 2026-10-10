# Module 06 — NVLink & InfiniBand Fabric

## 1. ပြဿနာ — ဘာကြောင့် အရေးကြီးသလဲ

AI training တွင် GPU များ တစ်လုံးတည်း မဟုတ်ဘဲ အဖွဲ့လိုက် အလုပ်လုပ်ပါသည် — GPU ကြား
ဆက်သွယ်ရေးသည် နှေးကွေးလျှင် GPU များ စောင့်နေရပြီး training တစ်ခုလုံး နှေးသွားပါသည်။
ထို့ကြောင့် ဆက်သွယ်ရေးလမ်းကြောင်း (fabric) ကို နားလည်ခြင်းသည် performance ပြဿနာ
ဖြေရှင်းရာတွင် အခြေခံ ဖြစ်ပါသည်။ တစ်ဖက်တွင်လည်း "NVLink = cluster fabric" ကဲ့သို့
ရှုပ်ထွေးမှုများကြောင့် စာမေးပွဲတွင် မှားတတ်သော အပိုင်း ဖြစ်ပါသည် — ကွဲပြားမှုကို
ရှင်းလင်းစွာ မှတ်သားထားပါ။

## 2. Core Concept — Scale-Up vs Scale-Out ⭐

```
┌─ Node (DGX) ──────────────────────────┐      ┌─ Cluster fabric ──────────┐
│  GPU0 ── NVLink ── GPU1               │      │                          │
│    │    NVSwitch    │                 │ IB   │   Switch (NVIDIA IB /    │
│  GPU2 ── NVLink ── GPU3               │◄────►│   Spectrum-X)            │
│         (intra-node, scale-up)        │ NIC  │   (inter-node, scale-out)│
└───────────────────────────────────────┘      └──────────────────────────┘
```

- **NVLink** = node အတွင်း GPU ↔ GPU ဆက်သွယ်ခြင်း (scale-up, intra-node) —
  bandwidth အမြင့်ဆုံး၊ NVSwitch ဖြင့် GPU အားလုံး အပြည့် ချိတ်ဆက်ပေးပါသည်။
- **InfiniBand/RoCE** = node ↔ node ဆက်သွယ်ခြင်း (scale-out, inter-node) —
  cluster တစ်ခုလုံးကို NIC နှင့် fabric switch များဖြင့် ချိတ်ဆက်ပါသည်။

စာမေးပွဲ trap — "NVLink = cluster fabric" ဟု ပြောလျှင် **မှား**ပါသည်။ ⭐
NVLink သည် node အတွင်းသာ ဖြစ်ပြီး၊ cluster fabric သည် InfiniBand/RoCE ဖြစ်ပါသည်။

## 3. NVLink မျိုးဆက်များ နှိုင်းယှဉ်ခြင်း ⭐

DGX ခေတ် မျိုးဆက်များ (3rd–6th gen) —

| Generation | System | Links | GPU ကြား BW |
|---|---|---|---|
| 3rd gen | DGX A100 | 12 | 600 GB/s |
| 4th gen | DGX H100/H200 | 18 | 900 GB/s |
| 5th gen | DGX B200/GB200 | 18 | 1.8 TB/s |
| 6th gen | Vera Rubin VR200 | 18 | 3.6 TB/s (HBM4 era) |

- link အရေအတွက် (12/18) ကို architecture ပြောင်းလျှင် simulator က လိုက်ပြောင်းပါသည်။
- NVLink 1st/2nd gen (Pascal/Volta ခေတ်) လည်း ရှိပြီး bandwidth ပိုနည်းပါသည် —
  လက်ရှိ DGX များတွင် 3rd gen အထက်သာ တွေ့ရပါမည်။

### InfiniBand link rate များ

| Generation | Rate (port လိုက်) | တွေ့ရလေ့ရှိသော system |
|---|---|---|
| HDR | 200 Gb/s | A100 မျိုးဆက် |
| NDR | 400 Gb/s | H100-B200 မျိုးဆက် |
| XDR | 800 Gb/s | GB200+ မျိုးဆက် |

## 4. Practical — ဖတ်နည်းများ ⭐

```bash
nvidia-smi nvlink -s       # link အလိုက် status (Active/Inactive) နှင့် bandwidth
nvidia-smi nvlink -e       # link error counters
nvidia-smi topo -m         # topology matrix (NV12/NV18/PIX/SYS)
```

- `nvlink -s` — link တစ်ခုချင်းစီ အလုပ်လုပ်/မလုပ် ကြည့်ရန်
- `nvlink -e` — error counter များဖြင့် link ပျက်စီးမှု အတိုင်းအတာ ခန့်မှန်းရန်
- `topo -m` — GPU ကြား ချိတ်ဆက်ပုံ (NV12/NV18 = NVLink 12/18 links, PIX = PCIe, SYS = system) ကြည့်ရန်

`nvidia-smi nvlink -s` output ဥပမာ (illustrative) —

```
GPU 0: NVLink 0: 26562 MB/s   (Active)   ← ပုံမှန်
GPU 0: NVLink 3: 0 MB/s       (Inactive) ← link ပျက်နေ!
```

### Fabric Manager

NVSwitch-based system (DGX) တွင် **nvidia-fabricmanager** service က NVLink fabric ကို
configure လုပ်ပေးပါသည် —

```bash
systemctl restart nvidia-fabricmanager
```

NVLink fabric ကို ပြန်တည်ဆောက်ပေးပါသည်။ service ရပ်နေလျှင် NVLink အလုပ်မလုပ်နိုင်ပါ —
link down ဖြစ်နေပါက ဤ service ကို ဦးစွာ စစ်ပါ။

### InfiniBand ဖတ်နည်း

```bash
ibstat                 # IB HCA state, rate, port info
```

output အဓိက အချက်များ —

- **State: Active** = link ကျန်းမာ (Down = ပြဿနာ)
- **Rate** — generation အလိုက် ကွာပါသည် (အပေါ်က ဇယား ကြည့်ပါ)
- **Physical state: LinkUp/LinkDown** — cable/transceiver ပြဿနာ ရှာရာတွင် အရေးကြီးပါသည်

## 5. Link Fault ဖြေရှင်းနည်း (Troubleshooting) ⭐

```
link down / degradation တွေ့
   ↓
① nvidia-smi nvlink -s / ibstat → ဘယ် link ဘယ် state
② error counters (-e) → ပျက်စီးမှု အတိုင်းအတာ
③ fabric manager service ရှိ/မရှိ
④ physical: reseat NVLink bridge / cable / transceiver
⑤ မကောင်းလျှင် replace → validate (topo + bandwidth)
```

software ဖြင့် စစ်ဆေးပြီးမှ physical intervention (reseat/replace) သို့ ဆင်းခြင်းကို
မှတ်သားထားပါ — ဤအစဉ်ကို module 07 နှင့် ဆက်စပ် လေ့လာပါ။

## 6. လုပ်ငန်းသုံး သတိပေးချက်များ (Operational Cautions)

- "NVLink = cluster fabric" မဟုတ်ပါ — NVLink = intra-node၊ InfiniBand/RoCE = inter-node။
- topology matrix ထဲက NV12/NV18 ကဲ့သို့ ဂဏန်းများကို ဖတ်တတ်ရန် လိုအပ်ပြီး
  generation ပြောင်းလျှင် link အရေအတွက် ပြောင်းနိုင်ပါသည်။
- `nvidia-smi` output format နှင့် field များသည် driver ဗားရှင်းပေါ် မူတည်၍ ကွာနိုင်ပါသည် —
  ဤနေရာက output ဥပမာသည် illustrative ဖြစ်ပါသည်။
- IB rate ဂဏန်းများသည် port လိုက် link rate ဖြစ်ပြီး system တစ်ခုလုံး၏ aggregate
  bandwidth မဟုတ်ပါ — ရှုပ်ထွေးမှု ရှောင်ပါ။

## 7. Self-Check

1. NVLink နှင့် InfiniBand ကွာခြားချက် (scope) ဘာလဲ။
   **အဖြေ** — NVLink = node အတွင်း GPU ↔ GPU (intra-node, scale-up)၊ InfiniBand/RoCE = node ↔ node (inter-node, scale-out) ဖြစ်ပါသည်။
2. DGX H100 ၏ NVLink link အရေအတွက် နှင့် GPU ကြား bandwidth?
   **အဖြေ** — 18 links ဖြင့် 900 GB/s (4th gen NVLink) ဖြစ်ပါသည်။
3. NVLink အလုပ်မလုပ်လျှင် ပထမ စစ်သင့်သော service ဘာလဲ။
   **အဖြေ** — `nvidia-fabricmanager` service — ရပ်နေလျှင် restart ပြုလုပ်ပါ။
4. link error ကို ဘယ် command ဖြင့် ကြည့်ရသလဲ။
   **အဖြေ** — `nvidia-smi nvlink -e` (link error counters) ဖြင့် ကြည့်နိုင်ပါသည်။

## 8. References

- NVLink: https://www.nvidia.com/en-us/data-center/nvlink/
- InfiniBand: https://docs.nvidia.com/networking/
- Fabric Manager: https://docs.nvidia.com/datacenter/tesla/fabric-manager-user-guide/

## Technical review notes

- 5th gen (1.8 TB/s) နှင့် 6th gen (3.6 TB/s per GPU) NVLink bandwidth များကို NVIDIA
  HGX/Vera Rubin စာရွက်စာတမ်းများဖြင့် အတည်ပြုပြီး ဇယားကို ဖြည့်စွက်ထားပါသည်။
  3rd/4th gen ဂဏန်းများ (600/900 GB/s, 12/18 links) ကို DGX A100/H100 documentation
  ဖြင့် အတည်ပြုပြီး။
- 6th gen (VR200) ၏ link အရေအတွက် 18 ဆိုသော အချက်ကို တိကျစွာ အတည်ပြု၍ မရသေးပါ —
  **needs verification**။
- XDR 800 Gb/s (GB200+) သည် port လိုက် rate ဖြစ်ပြီး မျိုးဆက်-system တွဲဆက်မှုသည်
  ယေဘုယျ လမ်းညွှန်ချက်ဖြစ်၍ **needs verification** (NIC/fabric ဖွဲ့စည်းပုံပေါ် မူတည်)။
- `nvidia-smi nvlink -s` output ဥပမာသည် မူရင်းအတိုင်း ထားသော illustrative output
  ဖြစ်ပြီး တကယ့် bandwidth ဂဏန်းများ မဟုတ်ပါ။
