# Module 09 — Solution (နမူနာ)

## E1 (XID 43)
- symptom: job ကပ်, XID 43 (GPU stopped responding)
- command: `dmesg | grep -i xid`, `nvidia-smi`
- root cause: GPU hang (SW/driver/clock)
- fix: `nvidia-smi --gpu-reset -i <gpu>` → မရ node reboot → ထပ်ဖြစ် RMA
- verify: `nvidia-smi` + `dcgmi diag -r 1`

## E2 (XID 79)
- symptom: GPU ပျောက်, XID 79 (fallen off bus)
- command: `dmesg | grep -i xid`, `nvidia-smi` (missing)
- root cause: PCIe/power/HW
- fix: Reseat GPU → `ipmitool chassis power cycle` → Mark for RMA
- verify: `nvidia-smi` တွင် GPU ပြန်မြင် + topo ပုံမှန်

## E3 (Thermal Alert)
- symptom: GPU အားလုံး 90-100°C, Perf throttle
- command: `nvidia-smi -q -d TEMPERATURE`, `ipmitool sensor list`, `nvsm show health`
- root cause: cooling/airflow/room (scope = system)
- fix: workload ရပ် → airflow/fan/CRAC စစ် → အေးမှ validate + ပြန်စ
- verify: temp ပုံမှန်ပြန်, Perf P0

## E4 (Severe ECC)
- symptom: uncorrectable ECC ထပ်တက်, XID 48/94
- command: `nvidia-smi -q -d ECC`, `dcgmi diag`
- root cause: HBM hardware
- fix: GPU isolate → Mark for RMA / replace
- verify: burn-in test + `dcgmi diag -r 2/3`

## E5
- XID 79 မျှော်လင့် — ပထမ ၂ ခု: ① `dmesg | grep -i xid` ဖြင့် ကုဒ် အတည်ပြု ② `nvidia-smi` ဖြင့် GPU 3 state/missing စစ်
