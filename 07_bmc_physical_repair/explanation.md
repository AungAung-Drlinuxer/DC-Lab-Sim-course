# Module 07 — BMC, OOB & Physical Repair

## 1. BMC / OOB / TPM ဘာတွေလဲ ⭐ (D1 — 31%)

| အရာ | အဓိပ္ပာယ် |
|---|---|
| **BMC** | Baseboard Management Controller — server ၏ သီးခြား mini-computer; OS မလိုဘဲ remote manage |
| **OOB** | Out-of-Band management — production network ပြင်ပ management network မှ စီမံခြင်း |
| **TPM** | Trusted Platform Module — hardware security chip (key, secure boot, attestation) |

BMC က power on/off, sensor ကြည့်, remote console (KVM), firmware update လုပ်နိုင်ပါသည် —
OS crash နေလည်း အလုပ်လုပ်ပါသည်။

## 2. ipmitool ⭐

```bash
ipmitool sensor list          # sensor အားလုံး (temp, voltage, fan)
ipmitool chassis power cycle  # server reboot (cold-ish)
ipmitool chassis power status
```
- `sensor list` — BMC မှ မြင်ရသော physical state အပြည့် — fan RPM, inlet temp, voltage
- `chassis power cycle` — OS မတက်တော့လျှင် remote reboot လုပ်ရာတွင် သုံး

## 3. dmidecode — hardware inventory ⭐

```bash
dmidecode        # BIOS, motherboard, memory, CPU, serial numbers
```
စက် တပ်ဆင်ခြင်း/asset မှတ်တမ်းတင်ခြင်းတွင် အသုံးပြုပါသည် —
"The Midnight Deployment" mission ကဲ့သို့ **စက်အသစ် bring-up** တွင် ပထမဆုံး စစ်ဆေးရန် tool ဖြစ်ပါသည်။

## 4. Bring-up / Deployment Sequence ⭐ (D1.1 — စာမေးပွဲ အမေးများ)

```
① Physical install   rack, power, network cable, transceiver
② BMC/OOB/TPM        management network, credentials, security
③ Firmware           BIOS/BMC/GPU firmware update (HGX အပါအဝင်)
④ Power/Cooling      parameter validate (PSU redundancy, airflow)
⑤ OS install         OS + driver (GPU/DOCA)
⑥ Network            interfaces, fabric validate
⑦ Hardware verify    nvidia-smi, dcgmi diag, burn-in
⑧ Cluster software   BCM/Slurm, workload လက်ခံ
```
⚠️ **sequence က ကိုယ်တိုင် မေးခွန်း ဖြစ်တတ်ပါသည်** — "next step" / "must be true before" ပုံစံ။

## 5. Physical Actions — ဘယ်အခါ ဘာလုပ် ⭐

| အခြေအနေ | လုပ်ဆောင်ချက် |
|---|---|
| command ဖြင့် ပြန်မကောင်း (reset မအောင်မြင်) | **Reseat GPU** |
| NVLink link ဆက်မကောင်း | **Reseat NVLink bridge** |
| ထပ်ခါတလဲ ပျက်စီး (hardware) | **Mark for RMA** (ပြန်လဲရန် မှာ) |

"software remediation ကုန်ပြီ" ဆိုမှ physical intervention အဆင့် ရောက်ပါသည် —
software → physical → replace အစဉ်ကို မှတ်ပါ။

## 6. Self-Check

1. BMC နှင့် OOB ကွာခြားချက်?
2. server OS မတက်တော့လျှင် remote reboot လုပ်သော command?
3. bring-up sequence တွင် firmware update က မည်သည့် အဆင့်လဲ။

**အဖြေ** — 1) BMC = chip/controller, OOB = management network approach · 2) `ipmitool chassis power cycle` · 3) ③ — physical install + BMC config ပြီးမှ

## 7. References

- DGX Administration: https://docs.nvidia.com/datacenter/dgx/
- BMC/IPMI: Intelligent Platform Management Interface spec
- NVIDIA HGX firmware: HGX user guide
