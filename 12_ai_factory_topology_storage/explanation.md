# Module 12 (Knowledge) — AI Factory Topology, Cabling & Storage

> D1 ၏ "network topologies for AI factories" + "cable/transceiver" + "third-party storage"
> ကို theory အဖြစ် အပြည့် ဖြည့်ပေးပါမည် — simulator ထဲ မပါသော ဗဟုသုတ။

---

## 1. ပြဿနာ — AI cluster ကို network design ကြိုပြီး ဘာကြောင့် ဆုံးဖြတ်ရသလဲ

AI training cluster တွင် GPU node များ ရာချီ ချိတ်ဆက်ရပါသည်။ ထိုစက်များကြား ဖြတ်သန်းသွားလာသော
traffic က အမျိုးအစား ၃ မျိုး ခွဲထွက်ပါသည် — GPU ချင်း အပြန်အလှန် (NCCL), node ↔ storage
(dataset read/write), နှင့် management (BMC/monitoring)။ traffic ၃ မျိုးလုံးကို network တစ်ခုတည်းပေါ်
ပေါင်းလိုက်ပါက NCCL all-reduce သည် storage I/O နှင့် ယှဉ်ပြီး congestion ဖြစ်ကာ GPU utilization
ကျဆင်းပါသည်။

ထို့ကြောင့် topology ရွေးချယ်မှု၊ cable/transceiver ရွေးချယ်မှု၊ storage architecture သည် cluster
တည်ဆောက်စဉ် ကတည်းက ဆုံးဖြတ်ရမည့် အချက်များ ဖြစ်ပါသည် — ကြိုးဆင်းများ မြေအောက်/rack ထဲ
ရောက်ပြီးမှ ပြန်ပြင်ရန် ခက်ခဲပါသည်။

## 2. Core Concept — AI Factory Network Topology ⭐

AI cluster တွင် network ၃ မျိုး ရှိပါသည် —

```
① Compute fabric (E/W)  — GPU node ချင်း (NCCL traffic) — InfiniBand/Spectrum-X
② Storage fabric (N/S)  — node ↔ storage (parallel FS)
③ Management fabric      — BMC/OOB, provision, monitoring
```

- **Compute fabric (East/West)** — NCCL collective traffic အဓိက — bandwidth အမြင့်ဆုံး လိုအပ်
- **Storage fabric (North/South)** — node များနှင့် storage ကြား parallel file system traffic
- **Management fabric** — BMC/out-of-band management, provisioning, monitoring — bandwidth နည်းသော်လည်း သီးခြား ထားရ

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

**ဘယ်အခါ သုံးမလဲ** — node အမြောက်အမြား ပါသော AI cluster အတွက် fat-tree + rail-optimized က
လက်ခံနိုင်သော standard ဖြစ်ပါသည်။ **အားနည်းချက်** — switch နှင့် cable အရေအတွက် များ၍
ကုန်ကျစရိတ် မြင့်ပါသည်။ leaf-spine အဆင့်တွင် oversubscription ရှိပါက non-blocking မဟုတ်တော့ဘဲ
workload ၏ bandwidth requirement နှင့် တွက်ချက် ဒီဇိုင်းဆွဲရပါမည်။

## 3. Cables & Transceivers ရွေးချယ်ခြင်း ⭐ (D1.8)

| အမျိုးအစား | အကွာအဝေး | အားသာချက် | အားနည်းချက် |
|---|---|---|---|
| **DAC** (copper) | 1-2m | ဈေးသက်သာ၊ power နည်း | တို၊ လေး |
| **AOC** (active optical) | 5-30m | ပေါ့၊ ရှည် | cable တစ်ခုတည်း (မလဲနိုင်) |
| **Optical transceiver + fiber** | 100m+ | အရှည်ဆုံး၊ transceiver လဲနိုင် | ဈေးမြင့် |

ရွေးချယ်ရာတွင် အဓိက အချက် ၂ ခု — **အကွာအဝေး** နှင့် **ကုန်ကျစရိတ်/ပြုပြင်နိုင်မှု**။
rack တစ်ခုအတွင်း DAC၊ rack ကြား AOC၊ building/campus အကွာအဝေး အတွက် optical transceiver + fiber က
ယေဘုယျ ရွေးချယ်ပုံ ဖြစ်ပါသည်။

validate = signal quality (error counter) + link state (`ibstat`) —
"signal quality ဖြင့် cable validate" = BER/error counter ကြည့်ခြင်း။ ⭐

## 4. Storage Architecture ⭐

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

- **Parallel file system (Lustre/GPFS)** — node အများ တစ်ပြိုင်နက် read နိုင် — training dataset အတွက် အဓိက tier
- **NVMe-oF / local NVMe** — scratch data အမြန် ရေး/ဖတ်
- **Object storage (S3)** — dataset archive/ရေရှည် သိုလှောင်ရုံ

storage test (D4.14) = read throughput တိုင်းပြီး training requirement နှင့် ကိုက်/မကိုက် စစ်ဆေးခြင်း —
GPU util နည်းခြင်း၏ အကြောင်းရင်းများထဲက data pipeline ကို တိုက်ရိုက် ပံ့ပိုးပါသည်။
(ရလဒ် ပမာဏက storage system နှင့် test pattern ပေါ် မူတည် — ဂဏန်း သတ်မှတ်ချက် မပေးပါ။)

## 5. Third-party Storage Initial Parameters (D1.11)

storage system တွင် — mount options, network config (IPoIB/RoCE), user mapping,
performance profile ကို ကြိုတင် သတ်မှတ်ရပါသည် — cluster software မထည့်မီ ပြင်ဆင်ရပါမည်။
parameter အသေးစိတ်က third-party storage vendor ပေါ် မူတည် — vendor documentation နှင့် ကိုက်ညီအောင်
စစ်ဆေးပါ။

## 6. Practical — link state ဘယ်လို စစ်မလဲ

```bash
ibstat    # InfiniBand port ၏ state (Active/Down) နှင့် link speed ကြည့်ရန်
```

- port state က Active ဖြစ်မှ cable/port ချိတ်ဆက်မှု မှန်ကန် — မဟုတ်ပါက physical cabling ပြန်စစ်ပါ
- error counter (BER) ကို NIC/switch ၏ vendor tool ဖြင့် ဖတ်ရှု — tool အမည်နှင့် command သည် vendor/OS
  ပေါ် မူတည်၍ ဤနေရာတွင် မသတ်မှတ်ပါ (needs verification)

## 7. လုပ်ငန်းသုံး သတိပြုရန် အချက်များ

- cable အမျိုးအစား ရွေးချယ်မှု မှားပါက distance/power budget ပြဿနာ နောက်မှ ဖြေရှင်းရ ခက်ပါသည်
- rail cabling မှားပါက NCCL traffic တစ် rail တည်းပေါ် စုကာ hotspot ဖြစ် — performance ကျဆင်းပါသည်
- storage throughput ကို ကြိုတင် မတိုင်းဘဲ workload ထည့်ပါက GPU util ကျပြီး အကြောင်းရင်း ရှာရ ခက်ပါသည်
- third-party storage parameter များ ကြိုမသတ်မှတ်ပါက cluster software install ပြီးမှ ပြန်ပြင်ရပါမည်

## 8. Self-Check

1. AI cluster ၏ network ၃ မျိုး က ဘာတွေလဲ?
2. rail-optimized topology က ဘာကို ကာကွယ်ပေးသလဲ?
3. 100m ကျော် အကွာအဝေးအတွက် ဘယ် cable type သုံးရမလဲ?
4. storage test က ဘာကို တိုင်းတာသလဲ?
5. third-party storage အတွက် ကြိုတင် သတ်မှတ်ရမည့် parameter ဥပမာ ပြောပြပါ။

**အဖြေ** — 1) compute fabric (E/W), storage fabric (N/S), management fabric ·
2) NCCL all-reduce traffic ကို path တစ်ခုတည်း မကျစေခြင်း (hotspot ကာကွယ်) ·
3) optical transceiver + fiber · 4) read/write throughput ကို training requirement နှင့် နှိုင်းယှဉ် စစ်ဆေးခြင်း ·
5) mount options, network config (IPoIB/RoCE), user mapping, performance profile

## 9. အခြား sources ရှင်းလင်းချက်

- **NVIDIA HGX AI Factory Reference Architecture** — logical/physical topology ၂ မျိုးလုံး
  ပုံစံချ ဖော်ပြ — စာမေးပွဲ D1.2 ၏ တရားဝင် အရင်းအမြစ်
- **Ravikiran guide** — storage topic ကို DGX SuperPOD storage architecture doc နှင့်
  ချိတ်ပေး — 3rd-party storage = "initial parameters" သီးခြား topic ဖြစ်ကြောင်း ဖော်ပြ

## 10. References

- HGX AI Factory: https://docs.nvidia.com/enterprise-reference-architectures/hgx-ai-factory/
- NVL72 AI Factory: https://docs.nvidia.com/enterprise-reference-architectures/nvl72-ai-factory/
- SuperPOD Storage: https://docs.nvidia.com/dgx-superpod/reference-architecture-scalable-infrastructure-h100/latest/storage-architecture.html
- LinkX: https://docs.nvidia.com/networking/display/CABLEOVpub

## Technical review notes

- DAC/AOC အကွာအဝေး တန်ဖိုးများ (1-2m, 5-30m, 100m+) သည် ယေဘုယျ တန်ဖိုးများ — product/vendor ပေါ် မူတည် (needs verification)
- error counter (BER) ဖတ်ရှုသော tool အမည်များ vendor/OS ပေါ် မူတည် (needs verification)
- ပြင်ဆင်မှု — self-check မေးခွန်း ၅ ခု ထည့်ပြီး link state စစ်ဆေးနည်း (`ibstat`) ကို practical section အဖြစ် ဖွဲ့စည်းခဲ့သည်။
