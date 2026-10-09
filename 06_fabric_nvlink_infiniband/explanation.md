# Module 06 — NVLink & InfiniBand Fabric

## 1. Scale-Up vs Scale-out — ဘယ်မှာ ဘာရှိသလဲ ⭐

```
┌─ Node (DGX) ──────────────────────────┐      ┌─ Cluster fabric ──────────┐
│  GPU0 ── NVLink ── GPU1               │      │                          │
│    │    NVSwitch    │                 │ IB   │   Switch (NVIDIA IB /    │
│  GPU2 ── NVLink ── GPU3               │◄────►│   Spectrum-X)            │
│         (intra-node, scale-up)        │ NIC  │   (inter-node, scale-out)│
└───────────────────────────────────────┘      └──────────────────────────┘
```
- **NVLink** = node အတွင်း GPU ↔ GPU (bandwidth အမြင့်ဆုံး)
- **InfiniBand/RoCE** = node ↔ node (cluster fabric)

စာမေးပွဲ trap — "NVLink = cluster fabric" ဟု ပြောလျှင် **မှား**ပါသည်။ ⭐

## 2. NVLink မျိုးဆက် ၆ ဆက် ⭐

| Generation | System | Links | GPU ကြား BW |
|---|---|---|---|
| 3rd gen | DGX A100 | 12 | 600 GB/s |
| 4th gen | DGX H100/H200 | 18 | 900 GB/s |
| 5th gen | DGX B200/GB200 | 18 | 1.8 TB/s |
| 6th gen | DGX VR200 | 18 | မြင့်မား (HBM4 era) |

link အရေအတွက် (12/18) ကို architecture ပြောင်းလျှင် simulator က လိုက်ပြောင်းပါသည်။

## 3. ဖတ်နည်း ⭐

```bash
nvidia-smi nvlink -s       # link အလိုက် status (Active/Inactive)
nvidia-smi nvlink -e       # link error counters
nvidia-smi topo -m         # topology matrix (NV12/NV18/PIX/SYS)
```

`nvidia-smi nvlink -s` output —
```
GPU 0: NVLink 0: 26562 MB/s   (Active)   ← ပုံမှန်
GPU 0: NVLink 3: 0 MB/s       (Inactive) ← link ပျက်နေ!
```

## 4. Fabric Manager ⭐

NVSwitch-based system (DGX) တွင် **nvidia-fabricmanager** service က
NVLink fabric ကို configure လုပ်ပေးပါသည် —
```bash
systemctl restart nvidia-fabricmanager
```
service ရပ်နေလျှင် NVLink အလုပ်မလုပ် — link down ဖြစ်နေပါက ဤ service ကို ဦးစွာ စစ်ပါ။

## 5. InfiniBand ဖတ်နည်း ⭐

```bash
ibstat                 # IB HCA state, rate, port info
```
output အဓိက အချက်များ —
- **State: Active** = link ကျန်းမာ (Down = ပြဿနာ)
- **Rate** — architecture အလိုက် ကွာ: HDR 200Gb/s (A100) / NDR 400Gb/s (H100-B200) / XDR 800Gb/s (GB200+)
- **Physical state: LinkUp/LinkDown** — cable/transceiver ပြဿနာ ရှာရာတွင် အရေးကြီး

## 6. Link Fault ဖြေရှင်းနည်း ⭐

```
link down / degradation တွေ့
   ↓
① nvidia-smi nvlink -s / ibstat → ဘယ် link ဘယ် state
② error counters (-e) → ပျက်စီးမှု အတိုင်းအတာ
③ fabric manager service ရှိ/မရှိ
④ physical: reseat NVLink bridge / cable / transceiver
⑤ မကောင်းလျှင် replace → validate (topo + bandwidth)
```

## 7. Self-Check

1. NVLink နှင့် InfiniBand ကွာခြားချက် (scope)?
2. DGX H100 ၏ NVLink link အရေအတွက်?
3. NVLink အလုပ်မလုပ်လျှင် ပထမ စစ်သင့်သော service?

**အဖြေ** — 1) intra-node vs inter-node · 2) 18 links (4th gen) · 3) nvidia-fabricmanager

## 8. References

- NVLink: https://www.nvidia.com/en-us/data-center/nvlink/
- InfiniBand: https://docs.nvidia.com/networking/
- Fabric Manager: https://docs.nvidia.com/datacenter/tesla/fabric-manager-user-guide/
