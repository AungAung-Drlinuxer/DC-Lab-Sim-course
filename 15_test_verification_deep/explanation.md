# Module 15 (Knowledge) — Cluster Test & Verification Deep (D4 — 33%)

> စာမေးပွဲ အကြီးဆုံး domain — HPL, NCCL, ClusterKit, burn-in, cable/FW validate
> ကို "ဘာကို သက်သေပြသလဲ" အလိုက် အပြည့်အစုံ ရှင်းပြပါမည်။

---

## 1. ပြဿနာ — "အလုပ်လုပ်တယ်" ဆိုတာ ဘယ်လို သက်သေပြမလဲ

cluster အသစ် တစ်ခုကို "အလုပ်လုပ်တယ်" ဟု မပြောနိုင်မီ — **တစ်ဆင့်ချင်း သက်သေပြ**ရပါမည်။
test တစ်ခုချင်းက component တစ်ခုချင်းကို ဖိအားပေးပြီး ကျန်းမာရေး စစ်ဆေးခြင်း ဖြစ်ပါသည်။
test တစ်ခုချင်းက ကိုယ်ပိုင် ရည်ရွယ်ချက် ရှိသဖြင့် "ဘယ် test က ဘာကို သက်သေပြသလဲ" ကို
ခွဲခြား သိရန် အရေးကြီးပါသည် — မှားယွင်းစွာ အဓိပ္ပာယ် ဖွင့်ပါက ပြဿနာ ဖုံးကွယ်မိပါမည်။

## 2. Core Concept — Verification Ladder

```
single-node → multi-node → burn-in (အချိန်ကြာ ဖိအား) → workload-ready
```

- **single-node** — GPU တစ်လုံးချင်း/node တစ်ခုချင်း အတွင်း စစ်ဆေး
- **multi-node** — fabric ကို စစ်ဆေး
- **burn-in** — အချိန်ကြာ ဖိအားပေးပြီး marginal component ဖော်ထုတ်
- **workload-ready** — end-to-end training path အောင်မြင်မှ ထည့်သုံးရန် အဆင်သင့်

## 3. Test Tools — ဘာကို သက်သေပြသလဲ ⭐⭐ (D4 ၏ နှလုံးသည်းချိန်)

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

## 4. HPL အသေးစိတ်

HPL = FP64 dense linear algebra solve —
- sustained compute + **thermal stability** (အချိန်ကြာ ဖိအားပေး)
- TFLOPS ရလဒ် ထွက် — DGX spec နှင့် နှိုင်းယှဉ် အတည်ပြု
- burn-in (4.12) = ထပ်ခါ ရှည်ချိန် run — marginal component ဖော်ထုတ်

## 5. NCCL Tests အသေးစိတ် ⭐

```bash
# single-node (NVLink Switch verify) — all_reduce bandwidth
mpirun -np 8 ./all_reduce_perf -b 8 -e 256M -f 2 -g 1

# E/W fabric (multi-node)
mpirun -np 16 --host ... ./all_reduce_perf ...
```

- `mpirun -np 8` — process ၈ ခု (GPU ၈ လုံး) ဖြင့် test run
- `./all_reduce_perf -b 8 -e 256M -f 2` — all_reduce bandwidth ကို message size 8B မှ 256MB အထိ တိုင်းတာ
- **single-node NCCL** = NVLink/NVSwitch path verify (D4.3)
- **E/W fabric bandwidth** = node ကြား InfiniBand/Spectrum-X verify (D4.10)
- **NCCL burn-in** (4.11) = ရှည်ချိန် run — intermittent link error ဖမ်း

ဤ command သည် nccl-tests ၏ ယေဘုယျ ဥပမာ — process အရေအတွက် (`-np`) နှင့် host list က
node/GPU အရေအတွက် ပေါ် မူတည်၍ ပြောင်းလဲ သတ်မှတ်ရပါမည်။

## 6. Cable & Firmware Validation ⭐

| စစ်ဆေးမှု | နည်းလမ်း |
|---|---|
| Cable signal quality (4.4) | link error counter / BER (`ibstat`, `nvidia-smi nvlink -e`) |
| Cabling correct (4.5) | topology map နှင့် ကိုက်/မကိုက် |
| Switch FW/SW (4.6) | `show version` (switch CLI) |
| BlueField-3 FW/SW (4.7) | `doca`/BFB version |
| Transceiver FW (4.8) | module info (ethtool/switch) |

ဤစစ်ဆေးမှုများက cable/FW level ပြဿနာ (link flap, version mismatch) ကို performance test
မလုပ်မီ ကြိုဖယ်ရှားပေးပါသည် — FW version များကို cluster version matrix နှင့် ကိုက်ညီအောင် စစ်ပါ။

## 7. ClusterKit ⭐

NVIDIA ClusterKit က node တစ်ခုချင်းစီကို **multi-faceted assessment** (CPU, GPU, memory,
network, storage) လုပ်ပေးပါသည် — cluster-wide validation အတွက် အစီအစဉ် ဖြစ်ပါသည်။
micro-benchmark တစ်ခုတည်း မဟုတ်ဘဲ node တစ်ခုချင်းစီ၏ ဘက်စုံ ကျန်းမာရေးကို တစ်နေရာတည်းမှ စစ်နိုင်ပါသည်။

## 8. Burn-in ဆိုတာ ဘာလဲ ⭐

burn-in = **အချိန်ကြာ ဖိအားပေး စစ်ဆေးခြင်း** — "infant mortality" (အသစ်
ချက်ချင်း ပျက်တတ်သော component) များကို production မဝင်မီ ဖော်ထုတ်ရန် —

```
HPL burn-in (compute) + NCCL burn-in (fabric) + NeMo burn-in (end-to-end)
```

## 9. လုပ်ငန်းသုံး သတိပြုရန် အချက်များ

- test တစ်ခုချင်း၏ ရလဒ်ကို "အားလုံး အောင်" ဟု မယူဘဲ scope အလိုက်သာ အဓိပ္ပာယ် ဖွင့်ပါ —
  micro-benchmark အောင်ပြီး workload မအောင် ဖြစ်နိုင်ပါသည်
- burn-in ကို production မဝင်မီ အချိန် လုံလောက်စွာ run ပါ — အချိန်တို run က intermittent fault ဖမ်းမမိပါ
- firmware version စစ်ဆေးမှုတွင် switch/BlueField/transceiver တစ်ခုချင်း version ကွဲနိုင် —
  version matrix နှင့် တစ်ခုချင်း ကိုက်ညီအောင် စစ်ပါ
- ရလဒ် ဂဏန်းများ (TFLOPS, bandwidth) က cluster config (power cap, clock, cable type) ပေါ်
  မူတည် — spec နှင့် နှိုင်းယှဉ်ရာတွင် config ကို အတူ မှတ်တမ်းတင်ပါ

## 10. Self-Check

1. HPL က ဘာကို သက်သေပြသလဲ?
2. single-node NCCL က NVLink ကို ဘယ်လို verify လုပ်သလဲ?
3. NeMo burn-in က micro-benchmark ထက် ဘာ ပိုစစ်သလဲ?
4. burn-in က ဘာကြောင့် လိုအပ်သလဲ?

**အဖြေ** — 1) sustained FP64 compute + thermal stability ·
2) single-node all-reduce bandwidth တိုင်းခြင်း (NVSwitch path verify) ·
3) storage/data pipeline အပါအဝင် end-to-end training path ·
4) "infant mortality" ဖြစ် component များကို production မဝင်မီ ဖော်ထုတ်ရန်

## 11. အခြား sources ရှင်းလင်းချက်

- **Ravikiran guide** — D4 topic တစ်ခုချင်းကို NVIDIA HPC Benchmarks docs နှင့် ချိတ်ပေး —
  "test တစ်ခုချင်းက ကိုယ်ပိုင် ရည်ရွယ်ချက် ရှိ" ဟူသော အခြေခံ ကို အလေးပေး
- **aguidetocloud.com** — "know what each validation tool uniquely proves" —
  HPL/NCCL/ClusterKit/NeMo ဇယားကို စာမေးပွဲ အတွက် အရေးကြီးဆုံး မှတ်စုအဖြစ် သတ်မှတ်

## 12. References & Further Reading

- NVIDIA HPC Benchmarks: https://docs.nvidia.com/nvidia-hpc-benchmarks
- Microbenchmarks: https://docs.nvidia.com/nvidia-hpc-benchmarks/Microbenchmarks.html
- NCCL: https://docs.nvidia.com/multi-node-nvlink-systems/multi-node-tuning-guide/nccl.html
- DCGM Diagnostics: https://docs.nvidia.com/datacenter/dcgm/latest/user-guide/dcgm-diagnostics.html

## Technical review notes

- NCCL test command သည် nccl-tests ၏ ဥပမာ — binary path၊ `mpirun -np` process အရေအတွက်နှင့်
  host list က cluster setup ပေါ် မူတည် (needs verification)။
- `nvidia-smi nvlink -e` ၏ output/format သည် GPU model နှင့် driver version ပေါ် မူတည် (needs verification)။
- HPL TFLOPS ကို DGX spec နှင့် နှိုင်းယှဉ်ရာတွင် ခွင့်ပြုနိုင်သော deviation ပမာဏက
  config (power cap, clock) ပေါ် မူတည် (needs verification)။
