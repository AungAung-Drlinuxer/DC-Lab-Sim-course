# Module 12 (Knowledge) — AI Factory Topology, Cabling & Storage

> D1 ၏ "network topologies for AI factories" + "cable/transceiver" + "third-party storage"
> ကို theory အဖြစ် အပြည့် ဖြည့်ပေးပါမည် — simulator ထဲ မပါသော ဗဟုသုတ။

---

## 1. AI Factory Network Topology ⭐

AI cluster တွင် network ၃ မျိုး ရှိပါသည် —

```
① Compute fabric (E/W)  — GPU node ချင်း (NCCL traffic) — InfiniBand/Spectrum-X
② Storage fabric (N/S)  — node ↔ storage (parallel FS)
③ Management fabric      — BMC/OOB, provision, monitoring
```

### Fat-tree / Rail-optimized topology ⭐

```
        ┌─────────┐  ┌─────────┐        spine layer
        │ switch  │  │ switch  │
        └────┬────┘  └────┬────┘
        ┌────┴────┐  ┌────┴────┐        leaf layer (rail)
   ┌────┤ switch  ├──┤ switch  ├────┐
   │    └────┬────┘  └────┬────┘    │
┌──┴──┐  ┌──┴──┐     ┌──┴──┐   ┌──┴──┐
│node0│  │node1│     │node2│   │node3│  ← GPU NIC (rail အလိုက်)
└─────┘  └─────┘     └─────┘   └─────┘
```

- **Rail-optimized** — node တစ်ခုရဲ့ NIC 0 အားလုံးက switch 0 သို့၊ NIC 1 က switch 1 သို့ —
  NCCL all-reduce traffic ကို path တစ်ခုတည်း မကျပါစေ (hotspot ကာကွယ်)
- **Fat-tree (Clos)** — bandwidth အဆင့်ဆင့် ညီမျှ — oversubscription 1:1 = non-blocking

## 2. Cables & Transceivers ⭐ (D1.8)

| အမျိုးအစား | အကွာအဝေး | အားသာချက် | အားနည်းချက် |
|---|---|---|---|
| **DAC** (copper) | 1-2m | ဈေးသက်သာ၊ power နည်း | တို၊ လေး |
| **AOC** (active optical) | 5-30m | ပေါ့၊ ရှည် | cable တစ်ခုတည်း (မလဲနိုင်) |
| **Optical transceiver + fiber** | 100m+ | အရှည်ဆုံး၊ transceiver လဲနိုင် | ဈေးမြင့် |

validate = signal quality (error counter) + link state (`ibstat`) —
"signal quality ဖြင့် cable validate" = BER/error counter ကြည့်ခြင်း။ ⭐

## 3. Storage Architecture ⭐

AI training အတွက် storage က **data ကို GPU မမီမီ ကျွေးနိုင်**ရမည် —

```
┌────────────────────────────────────┐
│  Parallel file system (Lustre/GPFS)│ ← အမြန်ဆုံး shared
│  NVMe-oF / local NVMe              │ ← scratch
│  Object storage (S3)               │ ← dataset archive
└────────────────────────────────────┘
          ↓ throughput validate
┌────────────────────────────────────┐
│  GPU nodes (dataloader)            │
└────────────────────────────────────┘
```

storage test (D4.14) = read throughput တိုင်းပြီး training requirement နှင့် ကိုက်/မကိုက် စစ်ဆေးခြင်း —
util နည်းခြင်း၏ အကြောင်းရင်း များထဲက data pipeline ကို တိုက်ရိုက် ပံ့ပိုးပါသည်။

## 4. Third-party Storage Initial Parameters (D1.11)

storage system တွင် — mount options, network config (IPoIB/RoCE), user mapping,
performance profile ကို ကြိုတင် သတ်မှတ်ရပါသည် — cluster software မထည့်မီ ပြင်ဆင်ရပါမည်။

## 5. အခြား sources ရှင်းလင်းချက်

- **NVIDIA HGX AI Factory Reference Architecture** — logical/physical topology ၂ မျိုးလုံး
  ပုံစံချ ဖော်ပြ — စာမေးပွဲ D1.2 ၏ တရားဝင် အရင်းအမြစ်
- **Ravikiran guide** — storage topic ကို DGX SuperPOD storage architecture doc နှင့်
  ချိတ်ပေး — 3rd-party storage = "initial parameters" သီးခြား topic ဖြစ်ကြောင်း ဖော်ပြ

## 6. References

- HGX AI Factory: https://docs.nvidia.com/enterprise-reference-architectures/hgx-ai-factory/
- NVL72 AI Factory: https://docs.nvidia.com/enterprise-reference-architectures/nvl72-ai-factory/
- SuperPOD Storage: https://docs.nvidia.com/dgx-superpod/reference-architecture-scalable-infrastructure-h100/latest/storage-architecture.html
- LinkX: https://docs.nvidia.com/networking/display/CABLEOVpub
