# Module 00 — Solution

## E1 နမူနာ ဖြေ
- မူလ dashboard: 8/8 Nodes Online, 64/64 GPUs Healthy, InfiniBand ✓ Active, BCM HA ✓
- dgx-00: 2× AMD EPYC 7742, 128 cores (64×2), RAM 1024GB, Driver 535.129.03, CUDA 12.2, Ubuntu 22.04.3, Slurm idle

## E2 မျှော်လင့်ရ
- A100→B200: GPU memory 80GB→192GB HBM3e, NVLink 3rd gen (12 links)→5th gen (18 links), HDR→NDR 400Gb/s
- Topology tab တွင် link အရေအတွက် architecture spec အတိုင်း ပြောင်း (A100=12, H100+=18)

## E3 output အဓိက အချက်များ
- `nvidia-smi` — GPU 0-7 စာရင်း, Temp/Pwr/Mem/Util ကောင်းနေလျှင် healthy
- `sinfo` — node state idle ဆိုလျှင် job လက်ခံနိုင်
- `ipmitool sensor list` — BMC sensor များ (temp/voltage/fan) ပုံမှန်

## E4
- `dmesg | grep -i xid` တွင် XID error code ပါသော kernel log ကြောင်း တွေ့ရမည် (Module 03 တွင် အသေးစိတ်)
- Reset ပြီး `nvidia-smi` တွင် GPU 0 state ပြန်ကောင်းပြီး error ပျောက်ရမည်
