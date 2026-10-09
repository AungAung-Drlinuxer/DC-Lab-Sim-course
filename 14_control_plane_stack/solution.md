# Module 14 — Solution

## E1
OS → GPU driver → DOCA driver → container toolkit → BCM/Slurm (Enroot/Pyxis) → NGC CLI

## E2
1. `nvidia-smi` (driver version)
2. `docker run --gpus all nvidia/cuda nvidia-smi`
3. `ngc --version`
4. `sinfo` (node state မြင်)

## E3
1. container ထဲ `nvidia-smi` run — GPU မြင်/မမြင်
2. host ပေါ် `nvidia-smi` — driver ကောင်း/မကောင်း
3. toolkit config/runtime — `nvidia-ctk runtime configure` + docker restart
