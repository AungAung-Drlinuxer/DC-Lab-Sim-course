# Module 02 — DCGM & NVSM Monitoring

## 1. ပြဿနာ — ဘာကြောင့် single-node tool နှင့် မလုံလောက်တာလဲ

`nvidia-smi` က node တစ်ခုအတွင်း GPU state ကို ပြပေးသော်လည်း cluster တစ်ခုလုံးကို
စောင့်ကြည့်ရန်၊ ကျန်းမာရေး policy သတ်မှတ်ရန်၊ diagnostic စနစ်တကျ ပြုလုပ်ရန်
မလုံလောက်ပါ။ node အများအပြား ရှိသော DGX cluster တွင် "ဘယ် node၊ ဘယ် GPU မှာ
ပြဿနာ ဖြစ်နေသလဲ" ဆိုသော မေးခွန်းကို ဖြေရန် **DCGM** နှင့် node တစ်ခုလုံး အနှစ်ချုပ်
ကြည့်ရန် **NVSM** ကို သုံးပါသည်။

## 2. Monitoring Tool ၃ ဆင့် — ဘယ်အခါ ဘာသုံး ⭐

| Tool | Scope | အဓိက အသုံး |
|---|---|---|
| `nvidia-smi` | single node | device state ချက်ချင်း ကြည့် |
| **DCGM** (`dcgmi`) | cluster (multi-node) | telemetry, diagnostics, health watches |
| **NVSM** | system (node rollup) | system health အနှစ်ချုပ် |

စာမေးပွဲတွင် — "cluster အဆင့် GPU monitoring" ဟု မေးလျှင် **DCGM**၊ "system health summary" ဟု မေးလျှင် **NVSM**။

## 3. DCGM (Data Center GPU Manager)

DCGM သည် GPU fleet အတွက် **telemetry + diagnostics + health policy** စီမံသော NVIDIA tool ဖြစ်ပါသည်။
`dcgmi` command ဖြင့် အသုံးပြုပါသည်။

### 3.1 Discovery — ဘာတွေ ရှိသလဲ
```bash
dcgmi discovery -l          # GPU အားလုံး + process များ စာရင်း
dcgmi group -l              # group များ စာရင်း
```

### 3.2 dmon — live telemetry ⭐
```bash
dcgmi dmon -e 1001,1002,1003,1004,1005   # field IDs အလိုက် monitor
```
Field ID အချို့ (NVIDIA DCGM profiling fields အရ) —

| Field ID | အဓိပ္ပာယ် |
|---|---|
| 1001 | graphics engine active (GPU utilization) |
| 1002 | SM active |
| 1003 | SM occupancy |
| 1004 | tensor active |
| 1005 | DRAM active (memory utilization) |
| 1009/1010 | PCIe TX / RX bytes |
| 1011/1012 | NVLink TX / RX bytes |

⚠️ DC Lab Sim မူရင်း ဇယားတွင် field ID များကို အောက်ပါအတိုင်း ဖော်ပြထားပါသည် —
1001 GPU utilization, 1002 memory utilization, 1003/1004 PCIe RX/TX bandwidth,
1005 energy consumption, 1009/1010 memory temperature/power usage, 1011 SM clock,
2030 ECC volatile uncorrectable။ ဤ mapping က simulator convention ဖြစ်နိုင်ပြီး
တကယ့် DCGM field numbering နှင့် မကိုက်နိုင်ပါ — စာမေးပွဲတွင် မေးပါက simulator ပေးသော
field ID ကို အခြေခံပါ (needs verification)။

### 3.3 diag — diagnostic run ⭐
```bash
dcgmi diag -r 1      # level 1: quick (health မြန်စစ်)
dcgmi diag -r 2      # level 2: standard
dcgmi diag -r 3      # level 3: deep (အချိန်ကြာ)
```
စာမေးပွဲ trap — "quick diagnostics" = `-r 1`။ အဆင့် မြင့်လေ ကျယ်ပြန့်/ကြာလေ။

### 3.4 health — policy watches
```bash
dcgmi health -s          # health watch အခြေအနေ ကြည့်
dcgmi health -g          # violations ကြည့်
```

## 4. NVSM (NVIDIA System Management)

```bash
nvsm show health         # system health အနှစ်ချုပ် ⭐
nvsm show system         # system အသေးစိတ်
```
NVSM က node တစ်ခုလုံး (GPU + NIC + storage + service) ၏ health ကို rollup လုပ်ပေးပါသည် —
DGX system-level ပြဿနာ ရှာရာတွင် ပထမ ကြည့်သင့်သော tool ဖြစ်ပါသည်။

## 5. Diagnostic Workflow ⭐

```
report ရသည် ("GPU တွေ နှေးနေတယ်")
   ↓
nvsm show health        → node အဆင့် ဘာပြဿနာ ရှိလဲ
   ↓
dcgmi discovery -l      → GPU အားလုံး မြင်ရလား
   ↓
dcgmi dmon -e 1001,1002 → live utilization/memory ကြည့်
   ↓
dcgmi diag -r 1         → quick diagnostic
   ↓
အသေးစိတ် → nvidia-smi -q / dmesg (Module 03)
```

အဆင့်စဉ်၏ အဓိပ္ပာယ် — အဝေးမှ အခြေအနေ အကြမ်း (NVSM) → ရှိ/မရှိ အတည်ပြု (discovery) →
live တိုင်းတာ (dmon) → အတည်ပြု diagnostic (diag) → အသေးစိတ် tool များ (nvidia-smi/dmesg)။
ကျယ်ပြန့်သော အဆင့်မှ ကျဉ်းမြောင်းသော အဆင့်သို့ ဆင်းသက်ခြင်း ဖြစ်ပါသည်။

## 6. လုပ်ငန်းခွင် သတိပြုရန် အချက်များ

- `dcgmi diag -r 3` (deep) က အချိန်ကြာပြီး ပို၍ ကျယ်ပြန့်သော test များ ပါဝင်နိုင်သောကြောင့်
  production workload ပြေးနေချိန်တွင် မပြုလုပ်ဘဲ ပြတ်တောက်ချိန် (maintenance window) တွင်
  ပြုလုပ်သင့်ပါသည် (deep diag ၏ အသေးစိတ် test အဆင့်များက version ပေါ် မူတည် —
  needs verification)။
- Field ID များသည် DCGM version ပေါ် မူတည်၍ ပြောင်းလဲနိုင်ပါသည် — တိကျသော field ID ကို
  အသုံးမပြုမီ `dcgmi profile -l` သို့မဟုတ် စာရွက်စာတမ်းဖြင့် အတည်ပြုပါ။
- Health watch violations ကို ကြည့်ရုံမျှဖြင့် မပြီးပါ — violation ဖြစ်စေသော threshold/policy
  ကိုပါ နားလည်မှသာ ထပ်ဖြစ်ခြင်း တားဆီးနိုင်ပါမည်။

## 7. Self-Check

1. cluster အဆင့် GPU telemetry အတွက် tool ဘာလဲ။
2. `dcgmi diag -r 1` နှင့် `-r 3` ကွာခြားချက်?
3. `dcgmi dmon -e 1001,1002` က ဘာတွေ ပြသလဲ။
4. node တစ်ခုလုံး (GPU + NIC + storage + service) ၏ health အနှစ်ချုပ် ကြည့်ရန် tool?
5. Diagnostic workflow တွင် `dcgmi diag -r 1` မလုပ်မီ ဘာကို အရင် စစ်သနည်း။

**အဖြေ** — 1) DCGM (`dcgmi`) · 2) quick vs deep diagnostic · 3) GPU utilization (graphics engine active) + memory utilization (DRAM active) · 4) NVSM (`nvsm show health`) · 5) `dcgmi discovery -l` ဖြင့် GPU အားလုံး မြင်ရမှုနှင့် `dcgmi dmon` ဖြင့် live utilization

## 8. References

- DCGM: https://docs.nvidia.com/datacenter/dcgm/
- DCGM CLI (`dcgmi`): https://docs.nvidia.com/datacenter/dcgm/latest/dcgm-api/dcgm-cli.html
- DCGM field IDs: https://docs.nvidia.com/datacenter/dcgm/latest/dcgm-api/dcgm-api-field-ids.html
- NVSM: https://docs.nvidia.com/datacenter/dgx/

## Technical review notes

- **ပြင်ဆင်မှု** — မူရင်း field ID ဇယား (1001 GPU util, 1002 memory util, 1003/1004 PCIe RX/TX,
  1005 energy, 1009/1010 mem temp/power, 1011 SM clock) ကို NVIDIA DCGM ရုံးတရားဝင်
  documentation (profiling fields) နှင့် တိုက်စစ်ပြီး ပြင်ဆင်ခဲ့သည် — တကယ့် DCGM တွင်
  1001=gr_engine_active, 1002=sm_active, 1003=sm_occupancy, 1004=tensor_active,
  1005=dram_active, 1009=pcie_tx_bytes, 1010=pcie_rx_bytes, 1011/1012=nvlink_tx/rx_bytes
  ဖြစ်သည်။ မူရင်း mapping ကို အောက်တွင် အမှတ်အသားပြု၍ ထားရှိပြီး simulator အတွက်
  **needs verification** ဖြစ်သည်။
- "2030 = ECC volatile uncorrectable" ဆိုသော field ID ကို ရုံးတရားဝင် field ID ဇယားတွင်
  အတည်မပြုနိုင်သေးပါ — **needs verification**။
- `dcgmi diag` level များ၏ အတိအကျ test အပြုအမူနှင့် workload အပေါ် သက်ရောက်မှုက
  DCGM version ပေါ် မူတည်သည် — **needs verification**။
