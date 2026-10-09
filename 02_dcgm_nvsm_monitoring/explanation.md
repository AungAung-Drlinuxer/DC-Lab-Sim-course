# Module 02 — DCGM & NVSM Monitoring

## 1. Monitoring Tool ၃ ဆင့် — ဘယ်အခါ ဘာသုံး ⭐

| Tool | Scope | အဓိက အသုံး |
|---|---|---|
| `nvidia-smi` | single node | device state ချက်ချင်း ကြည့် |
| **DCGM** (`dcgmi`) | cluster (multi-node) | telemetry, diagnostics, health watches |
| **NVSM** | system (node rollup) | system health အနှစ်ချုပ် |

စာမေးပွဲတွင် — "cluster အဆင့် GPU monitoring" ဟု မေးလျှင် **DCGM**၊ "system health summary" ဟု မေးလျှင် **NVSM**။

## 2. DCGM (Data Center GPU Manager)

DCGM သည် GPU fleet အတွက် **telemetry + diagnostics + health policy** စီမံသော NVIDIA tool ဖြစ်ပါသည်။
`dcgmi` command ဖြင့် အသုံးပြုပါသည်။

### 2.1 Discovery — ဘာတွေ ရှိသလဲ
```bash
dcgmi discovery -l          # GPU အားလုံး + process များ စာရင်း
dcgmi group -l              # group များ စာရင်း
```

### 2.2 dmon — live telemetry ⭐
```bash
dcgmi dmon -e 1001,1002,1003,1004,1005   # field IDs အလိုက် monitor
```
Field ID အချို့ —
| Field ID | အဓိပ္ပာယ် |
|---|---|
| 1001 | GPU utilization |
| 1002 | memory utilization |
| 1003 | PCIe RX bandwidth |
| 1004 | PCIe TX bandwidth |
| 1005 | total energy consumption |
| 1009/1010 | memory temperature / power usage |
| 1011 | SM clock |
| 2030 | ECC volatile uncorrectable |

### 2.3 diag — diagnostic run ⭐
```bash
dcgmi diag -r 1      # level 1: quick (health မြန်စစ်)
dcgmi diag -r 2      # level 2: standard
dcgmi diag -r 3      # level 3: deep (အချိန်ကြာ)
```
စာမေးပွဲ trap — "quick diagnostics" = `-r 1`။ အဆင့် မြင့်လေ ကျယ်ပြန့်/ကြာလေ။

### 2.4 health — policy watches
```bash
dcgmi health -s          # health watch အခြေအနေ ကြည့်
dcgmi health -g          # violations ကြည့်
```

## 3. NVSM (NVIDIA System Management)

```bash
nvsm show health         # system health အနှစ်ချုပ် ⭐
nvsm show system         # system အသေးစိတ်
```
NVSM က node တစ်ခုလုံး (GPU + NIC + storage + service) ၏ health ကို rollup လုပ်ပေးပါသည် —
DGX system-level ပြဿနာ ရှာရာတွင် ပထမ ကြည့်သင့်သော tool ဖြစ်ပါသည်။

## 4. Diagnostic Workflow ⭐

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

## 5. Self-Check

1. cluster အဆင့် GPU telemetry အတွက် tool ဘာလဲ။
2. `dcgmi diag -r 1` နှင့် `-r 3` ကွာခြားချက်?
3. `dcgmi dmon -e 1001,1002` က ဘာတွေ ပြသလဲ။

**အဖြေ** — 1) DCGM (`dcgmi`) · 2) quick vs deep diagnostic · 3) GPU utilization + memory utilization

## 6. References

- DCGM: https://docs.nvidia.com/datacenter/dcgm/
- DCGM CLI (`dcgmi`): https://docs.nvidia.com/datacenter/dcgm/latest/dcgm-api/dcgm-cli.html
- NVSM: https://docs.nvidia.com/datacenter/dgx/
