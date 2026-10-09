# Module 14 (Knowledge) — Control Plane Stack (D3 — 19%)

> BCM, OS, Slurm/Enroot/Pyxis, drivers, container toolkit, NGC CLI —
> cluster ကို software ဘက်မှ ဘယ်လို တပ်ဆင် စီမံသလဲ အပြည့်အစုံ ရှင်းပြပါမည်။

---

## 1. Control Plane ဆိုတာ ဘာလဲ (Background)

hardware (D1) ပြီးလျှင် **software stack** တပ်ဆင်ရပါမည် —
OS → drivers → cluster manager → scheduler → containers → workload tooling အစဉ်။

```
┌────────────────────────────────────────────┐
│  Workload tooling   NGC CLI, Pyxis/Enroot  │
├────────────────────────────────────────────┤
│  Scheduler          Slurm (+ Enroot/Pyxis) │
├────────────────────────────────────────────┤
│  Cluster manager    BCM (Base Command Mgr) │
├────────────────────────────────────────────┤
│  Drivers            GPU driver, DOCA, CUDA │
├────────────────────────────────────────────┤
│  Container runtime  NVIDIA container toolkit│
├────────────────────────────────────────────┤
│  OS                 Ubuntu/RHEL            │
└────────────────────────────────────────────┘
```

## 2. BCM (Base Command Manager) ⭐ (D3.1)

cluster တစ်ခုလုံးကို **တစ်နေရာတည်းမှ** provision/monitor/manage လုပ်သော NVIDIA software —
- **HA (High Availability)** — management node fail လျှင် failover — စာမေးပွဲတွင်
  "install BCM, configure and verify HA" ဟု သီးခြား topic ဖြစ် ⭐
- node provisioning, image management, health monitoring

## 3. OS + Drivers (D3.2, D3.4) ⭐

```
OS install → GPU driver (NVIDIA) → DOCA driver (BlueField) → CUDA
```
- **GPU driver** — `nvidia-smi` က version ပြ
- **DOCA driver** — DPU အတွက် (Module 13)
- driver update သည် kernel version နှင့် ဆက်စပ် — version matrix စစ်ပါ

## 4. Slurm + Enroot + Pyxis (D3.3) ⭐

- **Slurm** — scheduler (Module 08)
- **Enroot** — container runtime (HPC-optimized, rootless)
- **Pyxis** — Slurm plugin — `sbatch --container-image` ဖြင့် job ကို container အတွင်း run

```
sbatch --container-image=nvidia/cuda:12.x job.sh
        ↑ Pyxis → Enroot → container run on GPU node
```

## 5. NVIDIA Container Toolkit (D3.5, D3.6) ⭐

container က GPU ကို မြင်နိုင်စေသော component —
```bash
# install (Ubuntu)
distribution=$(. /etc/os-release;echo $ID$VERSION_ID)
curl -fsSL https://nvidia.github.io/libnvidia-container/gpgkey | sudo gpg --dearmor -o /usr/share/keyrings/nvidia-container-toolkit-keyring.gpg
sudo apt-get install -y nvidia-container-toolkit
sudo nvidia-ctk runtime configure --runtime=docker
sudo systemctl restart docker
```
```bash
docker run --gpus all nvidia/cuda:12.x nvidia-smi   # GPU ပြ/မပြ စမ်း ⭐
```
⚠️ `--gpus all` ဖြင့် run ပြီး `nvidia-smi` မြင်မှ ပြီးစီး — "demonstrate GPU with Docker"
ဆိုတာ ဒီ စမ်းသပ်ချက် ဖြစ်ပါသည်။

## 6. NGC CLI (D3.7) ⭐

NVIDIA NGC catalog မှ container/model ဆွဲရန် tool —
```bash
ngc --version
ngc config set ...        # API key
ngc registry image pull nvidia/cuda:12.x
```

## 7. အခြား sources ရှင်းလင်းချက်

- **Ravikiran guide** — D3 topic တစ်ခုချင်းကို official install guide နှင့် ချိတ်ပေး —
  "install → configure → verify" ၃ ဆင့် ပုံစံ လိုက်နာရန် အကြံပြု
- **aguidetocloud.com** — D3 ကို "sequence + verify" မေးခွန်း ဖြစ်နိုင် —
  install command ထက် အတည်ပြုနည်း ပိုမှတ်ရန်

## 8. Self-Check

1. BCM HA ဆိုတာ?
2. `--gpus all` ဖြင့် ဘာကို သက်သေပြသလဲ?
3. Pyxis က ဘာကို ပေါင်းပေးသလဲ?

**အဖြေ** — 1) management failover · 2) container toolkit GPU pass-through · 3) Slurm + container (Enroot)

## 9. References & Further Reading

- Container Toolkit Install: https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/1.16.2/install-guide.html
- Container Toolkit User Guide: https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/1.14.1/user-guide.html
- NGC User Guide: https://docs.nvidia.com/ngc/latest/ngc-user-guide.html
- BCM: https://docs.nvidia.com/base-command-manager/
