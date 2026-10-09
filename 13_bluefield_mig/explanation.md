# Module 13 (Knowledge) — BlueField DPU & MIG (D2 — 5%)

> D2 က weight ငယ်ပေမယ့် BlueField + MIG က ရှုပ်ထွေးပြီး ရမှတ် လွယ် —
> ဤ module တွင် concept + configuration + tools usage အပြည့် ရှင်းပြပါမည်။

---

## 1. BlueField DPU က ဘာလဲ (Background) ⭐

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

## 2. BlueField Modes of Operation ⭐⭐ (စာမေးပွဲ အမေးများ)

| Mode | ဘယ်လို အလုပ်လုပ် | ဘယ်အခါ |
|---|---|---|
| **Embedded/DPU mode** | ARM cores က infrastructure ကိုင် | ပြည့်စုံ offload |
| **NIC mode** | ConnectX NIC ကဲ့သို့သာ | offload မလို |
| **Separated host mode** | DPU/host သီးခြား စီမံ | multi-tenant |

configuration: DOCA ဖြင့် (`doca` command/API) — DOCA install က D3 ၏
"GPU နှင့် DOCA drivers" topic နှင့် ဆက်စပ်ပါသည်။

## 3. MIG (Multi-Instance GPU) ⭐⭐

GPU ကို **hardware-level partition** ခွဲခြင်း — SM/memory/cache သီးခြား။

![without MIG](../images/mig-multiuser-before.png)
*multi-user sharing (isolation မရှိ)*

![with MIG](../images/mig-multiuser-after.png)
*MIG instance များ (hardware-isolated)*

- Ampere+ data center GPU များတွင်သာ — instance **အမြင့်ဆုံး 7 ခု**
- QoS တည်ငြိမ် — inference workload မျှဝေအတွက် အကောင်းဆုံး

### MIG Configuration — Tools Usage ⭐

```bash
sudo nvidia-smi -mig 1                       # MIG mode enable (restart လို)
nvidia-smi -lgc / -lmc                       # profile များ စစ်
sudo nvidia-smi mig -cgi 9,9 -C              # instance ဖန်တီး
sudo nvidia-smi mig -lgi                     # instance စာရင်း
sudo nvidia-smi mig -dgi -gi 0               # instance ဖျက်
```

⚠️ **"enabled ≠ usable" trap** — `nvidia-smi -mig 1` က enable သာ —
instance ဖန်တီးမှ workload ထည့်နိုင် — စာမေးပွဲက ဒုတိယ အဆင့်ကို သက်သေပြ command မေးလေ့ရှိ။ ⭐

### MIG Profile (H100 ဥပမာ)

| Profile | Memory | instances |
|---|---|---|
| 1g.10gb | 10GB | 8 (သို့) 7* |
| 2g.20gb | 20GB | 4 |
| 3g.40gb | 40GB | 2 |
| 7g.80gb | 80GB | 1 |
*GPU generation ပေါ် မူတည် — "7 instances" ကို exam answer အဖြစ် မှတ်

## 4. အခြား sources ရှင်းလင်းချက်

- **NVIDIA MIG User Guide** — "MIG mode" နှင့် "instance" ကို concept ၂ ခုအဖြစ် ခွဲ ဖော်ပြ
- **aguidetocloud.com** — "enabled vs usable" trap ကို MIG ၏ သီးခြား ဥပမာအဖြစ် အလေးပေး
- **Ravikiran guide** — BlueField topic ကို DOCA docs နှင့် ချိတ် — mode of operation
  ကို "platform management" အဖြစ် မြင်ရန် အကြံပြု

## 5. Self-Check

1. DPU က CPU ကနေ ဘာတွေ ဖယ်ရှားပေးသလဲ?
2. MIG enable လုပ်ပြီး workload ထည့်နိုင်ပြီလား?
3. DOCA က ဘာအတွက် platform?

**အဖြေ** — 1) network/storage/security · 2) မရသေး — instance ဖန်တီးမှ (enabled ≠ usable) · 3) DPU

## 6. References & Further Reading

- BlueField/DOCA: https://docs.nvidia.com/networking/dpu-doca/
- DOCA Install: https://docs.nvidia.com/doca/archive/2-9-2/doca+installation+guide+for+linux/
- MIG User Guide: https://docs.nvidia.com/datacenter/tesla/mig-user-guide/
- MIG Profiles: https://docs.nvidia.com/datacenter/tesla/mig-user-guide/supported-mig-profiles.html
