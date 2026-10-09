# Module 01 — Solution

## E1 နမူနာ
- ပုံမှန် node: Temp 30-45°C, Pwr 250-320W/400W, Mem 0-200MiB (idle), Util 0%
- `nvidia-smi -q`: Persistence Mode = Enabled, Compute Mode = Default
- topo -m: DGX တွင် GPU 0-1 = NVLink (NV18/NV12 အလိုက် architecture)

## E2 မျှော်လင့်ရ ပုံစံ
| Workload | Util | Mem | Pwr | Temp |
|---|---|---|---|---|
| Idle | ~5% | ~50-200MB | ~15% TDP | ~ambient |
| Inference | ~60% | အလယ် | အလယ် | 40-60°C |
| Training | ~95% | မြင့် | မြင့် | 60-80°C |
| Stress | 100% | အမြင့် | cap အထိ | 70-85°C |

## E3
- `nvidia-smi -q -d TEMPERATURE` တွင် GPU temp 90°C+ တက်လာ
- Perf column က `P0` မှ `P2/P3` သို့ ပြောင်း (throttle)
- Reset ပြီးနောက် temp ပုံမှန် ပြန်ဆင်း၊ Perf P0 ပြန်
