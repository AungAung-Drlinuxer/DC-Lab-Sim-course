# Module 08 — Solution

## E1
- မူလ: idle — Training apply ပြီး: alloc/mix (workload က သုံးနေ)

## E2 နမူနာ ဇယား
| Workload | Util | Mem | Pwr | Temp |
|---|---|---|---|---|
| Idle | 5% | ~100MB | ~60W | ~35°C |
| Inference | 60% | အလယ် | ~240W | ~50°C |
| Training | 95% | မြင့် | ~380W | ~70°C |
| Stress | 100% | အမြင့် | ~400W (cap) | ~80°C |

- Stress Test က thermal အနီးစပ်ဆုံး

## E3
- command: `sinfo` (workload/node state), `nvidia-smi` (util + Perf), `nvidia-smi -q -d TEMPERATURE/POWER` (throttle?)
- ဖြစ်နိုင်ချေ: ① data loading/CPU bottleneck (GPU စောင့်နေ) ② power/thermal throttle ③ small batch/config ကြောင့် GPU မပြည့်
