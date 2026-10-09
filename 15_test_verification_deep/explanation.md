# Module 15 (Knowledge) — Cluster Test & Verification Deep (D4 — 33%)

> စာမေးပွဲ အကြီးဆုံး domain — HPL, NCCL, ClusterKit, burn-in, cable/FW validate
> ကို "ဘာကို သက်သေပြသလဲ" အလိုက် အပြည့်အစုံ ရှင်းပြပါမည်။

---

## 1. Verification Philosophy (Background)

cluster အသစ် တစ်ခုကို "အလုပ်လုပ်တယ်" ဟု မပြောနိုင်မီ —
**တစ်ဆင့်ချင်း သက်သေပြ**ရပါမည်။ test တစ်ခုချင်းက component တစ်ခုချင်းကို ဖိအားပေးပြီး
ကျန်းမာရေး စစ်ဆေးခြင်း ဖြစ်ပါသည်။

```
single-node → multi-node → burn-in (အချိန်ကြာ ဖိအား) → workload-ready
```

## 2. Test Tools — ဘာကို သက်သေပြသလဲ ⭐⭐ (D4 ၏ နှလုံးသည်းချိန်)

| Test | သက်သေပြမှု | D4 topic |
|---|---|---|
| Single-node stress | GPU compute + memory + thermal တည်ငြိမ် | 4.1 |
| **HPL** (Linpack) | sustained FP64 compute + cooling | 4.2, 4.12 |
| **NCCL test** | GPU ကြား/node ကြား collective bandwidth (NVLink Switch verify ပါ) | 4.3, 4.10, 4.11 |
| **ClusterKit** | node-to-node multifaceted assessment | 4.9 |
| **NeMo burn-in** | end-to-end training path (storage/data pipeline ပါ) | 4.13 |
| Storage test | read/write throughput | 4.14 |

⚠️ micro-benchmark (HPL/NCCL) က **storage/data path ကို မစစ်**ပါ —
NeMo burn-in ကသာ end-to-end အပြည့် ဖြစ်ပါသည်။ ⭐

## 3. HPL အသေးစိတ်

HPL = FP64 dense linear algebra solve —
- sustained compute + **thermal stability** (အချိန်ကြာ ဖိအားပေး)
- TFLOPS ရလဒ် ထွက် — DGX spec နှင့် နှိုင်းယှဉ် အတည်ပြု
- burn-in (4.12) = ထပ်ခါ ရှည်ချိန် run — marginal component ဖော်ထုတ်

## 4. NCCL Tests အသေးစိတ် ⭐

```bash
# single-node (NVLink Switch verify) — all_reduce bandwidth
mpirun -np 8 ./all_reduce_perf -b 8 -e 256M -f 2 -g 1

# E/W fabric (multi-node)
mpirun -np 16 --host ... ./all_reduce_perf ...
```
- **single-node NCCL** = NVLink/NVSwitch path verify (D4.3)
- **E/W fabric bandwidth** = node ကြား InfiniBand/Spectrum-X verify (D4.10)
- **NCCL burn-in** (4.11) = ရှည်ချိန် run — intermittent link error ဖမ်း

## 5. Cable & Firmware Validation ⭐

| စစ်ဆေးမှု | နည်းလမ်း |
|---|---|
| Cable signal quality (4.4) | link error counter / BER (`ibstat`, `nvidia-smi nvlink -e`) |
| Cabling correct (4.5) | topology map နှင့် ကိုက်/မကိုက် |
| Switch FW/SW (4.6) | `show version` (switch CLI) |
| BlueField-3 FW/SW (4.7) | `doca`/BFB version |
| Transceiver FW (4.8) | module info (ethtool/switch) |

## 6. ClusterKit ⭐

NVIDIA ClusterKit က node တစ်ခုချင်းစီကို **multi-faceted assessment** (CPU, GPU, memory,
network, storage) လုပ်ပေးပါသည် — cluster-wide validation အတွက် အစီအစဉ် ဖြစ်ပါသည်။

## 7. Burn-in ဆိုတာ ဘာလဲ ⭐

burn-in = **အချိန်ကြာ ဖိအားပေး စစ်ဆေးခြင်း** — "infant mortality" (အသစ်
ချက်ချင်း ပျက်တတ်သော component) များကို production မဝင်မီ ဖော်ထုတ်ရန် —
```
HPL burn-in (compute) + NCCL burn-in (fabric) + NeMo burn-in (end-to-end)
```

## 8. အခြား sources ရှင်းလင်းချက်

- **Ravikiran guide** — D4 topic တစ်ခုချင်းကို NVIDIA HPC Benchmarks docs နှင့် ချိတ်ပေး —
  "test တစ်ခုချင်းက ကိုယ်ပိုင် ရည်ရွယ်ချက် ရှိ" ဟူသော အခြေခံ ကို အလေးပေး
- **aguidetocloud.com** — "know what each validation tool uniquely proves" —
  HPL/NCCL/ClusterKit/NeMo ဇယားကို စာမေးပွဲ အတွက် အရေးကြီးဆုံး မှတ်စုအဖြစ် သတ်မှတ်

## 9. Self-Check

1. HPL က ဘာကို သက်သေပြသလဲ?
2. single-node NCCL က NVLink ကို ဘယ်လို verify လုပ်သလဲ?
3. NeMo burn-in က micro-benchmark ထက် ဘာ ပိုစစ်သလဲ?

**အဖြေ** — 1) sustained FP64 + thermal · 2) all-reduce bandwidth (NVSwitch path) · 3) storage/data pipeline (end-to-end)

## 10. References & Further Reading

- NVIDIA HPC Benchmarks: https://docs.nvidia.com/nvidia-hpc-benchmarks
- Microbenchmarks: https://docs.nvidia.com/nvidia-hpc-benchmarks/Microbenchmarks.html
- NCCL: https://docs.nvidia.com/multi-node-nvlink-systems/multi-node-tuning-guide/nccl.html
- DCGM Diagnostics: https://docs.nvidia.com/datacenter/dcgm/latest/user-guide/dcgm-diagnostics.html
