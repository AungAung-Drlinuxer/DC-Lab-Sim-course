# Module 14 (Knowledge) — Control Plane Stack (D3 — 19%)

> BCM, OS, Slurm/Enroot/Pyxis, drivers, container toolkit, NGC CLI —
> cluster ကို software ဘက်မှ ဘယ်လို တပ်ဆင် စီမံသလဲ အပြည့်အစုံ ရှင်းပြပါမည်။

---

## 1. ပြဿနာ — hardware ပြီးရုံနှင့် cluster အလုပ်လုပ်ပြီ မဟုတ်

D1 အတိုင်း hardware တပ်ဆင်ပြီးပါက နောက်တစ်ဆင့်မှာ **software stack** —
OS → drivers → cluster manager → scheduler → containers → workload tooling ကို
အစဉ်လိုက် တပ်ဆင်ရပါမည်။ အစဉ်မှားပါက (ဥပမာ driver မတပ်မီ container toolkit တပ်)
ပြန်ဖျက် ပြန်တပ် လုပ်ရပါမည်။ D3 သည် စာမေးပွဲ weight 19% ရှိသဖြင့် component တစ်ခုချင်း၏
အခန်းကဏ္ဍနှင့် "install → configure → verify" အစဉ်ကို သေချာ မှတ်ထားပါ။

## 2. Core Concept — Control Plane Stack အဆင့်များ

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

component တစ်ခုချင်း၏ အခန်းကဏ္ဍ —

| Component | အလုပ် | အောက်ပါ section |
|---|---|---|
| OS (Ubuntu/RHEL) | node အခြေခံ platform | §4 |
| GPU driver / DOCA / CUDA | hardware access layer | §4 |
| NVIDIA container toolkit | container က GPU မြင်စေ | §6 |
| BCM | cluster provision/monitor/manage | §3 |
| Slurm + Enroot + Pyxis | job schedule + container run | §5 |
| NGC CLI | container/model ဆွဲယူ | §7 |

## 3. BCM (Base Command Manager) ⭐ (D3.1)

cluster တစ်ခုလုံးကို **တစ်နေရာတည်းမှ** provision/monitor/manage လုပ်သော NVIDIA software —
- **HA (High Availability)** — management node fail လျှင် failover — စာမေးပွဲတွင်
  "install BCM, configure and verify HA" ဟု သီးခြား topic ဖြစ် ⭐
- node provisioning, image management, health monitoring

**ဘယ်အခါ သုံးမလဲ** — node အမြောက်အမြား ရှိသော cluster ကို လက်ဖြင့် တစ်လုံးချင်း မစီမံချင်ပါက။
HA မရှိပါက management node fail လျှင် cluster စီမံခန့်ခွဲမှု တစ်ခုလုံး ရပ်သွားပါမည်။

## 4. OS + Drivers (D3.2, D3.4) ⭐

```
OS install → GPU driver (NVIDIA) → DOCA driver (BlueField) → CUDA
```

- **GPU driver** — `nvidia-smi` က version ပြ (install အောင်မြင်ကြောင်း ပထမဆုံး အတည်ပြုနိုင်သော command)
- **DOCA driver** — DPU အတွက် (Module 13)
- **CUDA** — GPU compute platform/toolkit
- driver update သည် kernel version နှင့် ဆက်စပ် — version matrix စစ်ပါ

## 5. Slurm + Enroot + Pyxis (D3.3) ⭐

- **Slurm** — scheduler (Module 08)
- **Enroot** — container runtime (HPC-optimized, rootless)
- **Pyxis** — Slurm plugin — `sbatch --container-image` ဖြင့် job ကို container အတွင်း run

```
sbatch --container-image=nvidia/cuda:12.x job.sh
        ↑ Pyxis → Enroot → container run on GPU node
```

အလုပ်လုပ်ပုံ — Pyxis က Slurm job ကို container image ဖြင့် လက်ခံပြီး Enroot က node ပေါ်တွင်
container ဖွင့်ကာ GPU node ပေါ် run ပေးပါသည်။ ဤနည်းဖြင့် job တစ်ခုချင်း၏ software environment
ကို image အဖြစ် သတ်မှတ်နိုင်ပါသည်။

## 6. NVIDIA Container Toolkit (D3.5, D3.6) ⭐ — Practical Configuration

container က GPU ကို မြင်နိုင်စေသော component။ Ubuntu/apt တွင် တပ်ဆင်ပုံ
(official install guide အတိုင်း) —

```bash
# 1. NVIDIA container toolkit repository ချိတ် (GPG key + apt source list)
curl -fsSL https://nvidia.github.io/libnvidia-container/gpgkey | sudo gpg --dearmor -o /usr/share/keyrings/nvidia-container-toolkit-keyring.gpg \
     && curl -s -L https://nvidia.github.io/libnvidia-container/stable/deb/nvidia-container-toolkit.list | \
       sed 's#deb https://#deb [signed-by=/usr/share/keyrings/nvidia-container-toolkit-keyring.gpg] https://#g' | \
       sudo tee /etc/apt/sources.list.d/nvidia-container-toolkit.list

# 2. repository မှ package list update
sudo apt-get update

# 3. toolkit တပ်ဆင်
sudo apt-get install -y nvidia-container-toolkit

# 4. Docker runtime ကို NVIDIA runtime ဖြင့် configure (daemon.json ပြင်ပေး)
sudo nvidia-ctk runtime configure --runtime=docker

# 5. Docker daemon ပြန်ဖွင့် (configure ဝင်စေရန်)
sudo systemctl restart docker
```

```bash
docker run --gpus all nvidia/cuda:12.x nvidia-smi   # container ထဲမှ GPU မြင်/မမြင် စမ်းသပ် ⭐
```

- `nvidia-ctk runtime configure` — `/etc/docker/daemon.json` ကို ပြင်ပေးပြီး Docker က
  NVIDIA container runtime သုံးစေ
- `--gpus all` ဖြင့် run ပြီး `nvidia-smi` မြင်မှ GPU pass-through အောင်မြင် —
  "demonstrate GPU with Docker" ဆိုတာ ဒီ စမ်းသပ်ချက် ဖြစ်ပါသည်။ ⭐

ဤ command များသည် Ubuntu/apt environment အတွက် ဥပမာ — yum/dnf/zypper သုံး distro
များတွင် repository setup command ကွဲပါမည်။

## 7. NGC CLI (D3.7) ⭐

NVIDIA NGC catalog မှ container/model ဆွဲရန် tool —

```bash
ngc --version                              # NGC CLI တပ်ဆင်မှု အတည်ပြု
ngc config set ...                         # API key နှင့် org/team သတ်မှတ်
ngc registry image pull nvidia/cuda:12.x   # catalog မှ image ဆွဲယူ
```

## 8. လုပ်ငန်းသုံး သတိပြုရန် အချက်များ

- driver version နှင့် kernel version ကိုက်ညီမှု မစစ်ဘဲ driver update မလုပ်ရ — boot မဝင်နိုင်သည်အထိ ဖြစ်နိုင်
- container toolkit တပ်ပြီး Docker restart မလုပ်ပါက configure မဝင် — `--gpus` စမ်းသပ်ချက် ကျရှုံးပါမည်
- `docker run --gpus all` စမ်းသပ်ချက် မအောင်မြင်ပါက install → configure → restart အဆင့် တစ်ဆင့်ချင်း ပြန်စစ်ပါ
- NGC API key ကို `ngc config set` ဖြင့် သိမ်းထားသဖြင့် shared machine တွင် key မျှဝေမိမှု သတိပြုပါ
- version pin လုပ်ထားသော doc/command များသည် version အသစ်တွင် ပြောင်းလဲနိုင် — release note စစ်ပါ

## 9. Self-Check

1. BCM HA ဆိုတာ?
2. `--gpus all` ဖြင့် ဘာကို သက်သေပြသလဲ?
3. Pyxis က ဘာကို ပေါင်းပေးသလဲ?
4. container toolkit တပ်ပြီး Docker တွင် GPU သုံးနိုင်ကြောင်း ဘယ်လို အတည်ပြုမလဲ?

**အဖြေ** — 1) management node fail လျှင် failover (management ဆက်လက် အလုပ်လုပ်နိုင်ရေး) ·
2) container က GPU ကို မြင်နိုင်ခြင်း (GPU pass-through) · 3) Slurm + container (Enroot) ·
4) `docker run --gpus all nvidia/cuda:12.x nvidia-smi` ဖြင့် run ပြီး container ထဲတွင် GPU မြင်ကြည့်ခြင်း

## 10. အခြား sources ရှင်းလင်းချက်

- **Ravikiran guide** — D3 topic တစ်ခုချင်းကို official install guide နှင့် ချိတ်ပေး —
  "install → configure → verify" ၃ ဆင့် ပုံစံ လိုက်နာရန် အကြံပြု
- **aguidetocloud.com** — D3 ကို "sequence + verify" မေးခွန်း ဖြစ်နိုင် —
  install command ထက် အတည်ပြုနည်း ပိုမှတ်ရန်

## 11. References & Further Reading

- Container Toolkit Install: https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/1.16.2/install-guide.html
- Container Toolkit User Guide: https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/1.14.1/user-guide.html
- NGC User Guide: https://docs.nvidia.com/ngc/latest/ngc-user-guide.html
- BCM: https://docs.nvidia.com/base-command-manager/

## Technical review notes

- ပြင်ဆင်မှု — မူလ container toolkit install snippet တွင် repository setup (apt source list) နှင့်
  `apt-get update` အဆင့် ပါမလာသဖြင့် official install guide (v1.16.2) အတိုင်း ဖြည့်စွက်ခဲ့သည်။
  မူလ `distribution=$(...)` line ကို ဖယ်ရှားခဲ့သည် (snippet အတွင်း အသုံးမပြု)။
- ပြင်ဆင်မှု — `ngc config set ...` ဥပမာတွင် parameter အမည် မဖော်ပြထား — API key/org/team
  အသေးစိတ်ကို NGC User Guide ဖြင့် အတည်ပြုပါ (needs verification)။
- reference link များ version pin (1.16.2/1.14.1) ဖြစ် — command အသေးစိတ်နှင့် behavior က
  version ပေါ် မူတည် (needs verification)။
