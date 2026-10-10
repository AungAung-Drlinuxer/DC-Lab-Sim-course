# Module 07 — BMC, OOB & Physical Repair

## 1. ပြဿနာ — ဘာကြောင့် အရေးကြီးသလဲ

server တစ်လုံး OS မတက်တော့ခြင်း၊ GPU ပျက်စီးခြင်း ဖြစ်လာလျှင် datacenter အတွင်း
မှ မည်သို့ ဝေးကွာစွာ စီမံမလဲ — ထို့အပြင် စက်အသစ် တပ်ဆင်ချိန်တွင် အဆင့်များ မှားလျှင်
နောက်ပိုင်း ပြန်လည် ပြင်ဆင်ရခြင်း ဖြစ်တတ်ပါသည်။ BMC နှင့် OOB management က
OS ပေါ် မမူတည်ဘဲ စက်ကို ဝေးကွာစွာ ထိန်းချုပ်နိုင်စေပြီး၊ bring-up sequence က
စက်အသစ်တစ်ခု လုပ်ငန်းဝင်စေရေး အတွက် လမ်းပြမြေပုံ ဖြစ်ပါသည် (စာမေးပွဲ D1 တွင်လည်း
အလေးပေး မေးလေ့ ရှိပါသည်)။

## 2. Core Concept — BMC / OOB / TPM ⭐ (D1 — 31%)

| အရာ | အဓိပ္ပာယ် |
|---|---|
| **BMC** | Baseboard Management Controller — server ၏ သီးခြား mini-computer; OS မလိုဘဲ remote manage |
| **OOB** | Out-of-Band management — production network ပြင်ပ management network မှ စီမံခြင်း |
| **TPM** | Trusted Platform Module — hardware security chip (key, secure boot, attestation) |

BMC သည် server motherboard ပေါ်ရှိ သီးခြား controller ဖြစ်ပြီး ၎င်း၏ ကိုယ်ပိုင် network နှင့်
power ရှိပါသည် — ထို့ကြောင့် main OS crash နေလည်း အလုပ်လုပ်ပါသည်။ power on/off,
sensor ကြည့်ခြင်း၊ remote console (KVM)၊ firmware update များ လုပ်နိုင်ပါသည်။
OOB ဆိုသည်မှာ BMC ကဲ့သို့ tool မဟုတ်ဘဲ production network မှ ခွဲထားသော management
network မှတစ်ဆင့် စီမံခြင်း **နည်းလမ်း** ဖြစ်ပါသည်။

## 3. လုပ်ဆောင်ချက် အဆင့် နှိုင်းယှဉ်ခြင်း — ဘယ်အခါ ဘာလုပ် ⭐

software remediation ကုန်ပြီးမှသာ physical intervention အဆင့် ရောက်ပါသည် —
**software → physical → replace** အစဉ်ကို မှတ်ပါ။

| အခြေအနေ | လုပ်ဆောင်ချက် |
|---|---|
| command ဖြင့် ပြန်မကောင်း (reset မအောင်မြင်) | **Reseat GPU** |
| NVLink link ဆက်မကောင်း | **Reseat NVLink bridge** |
| ထပ်ခါတလဲ ပျက်စီး (hardware) | **Mark for RMA** (ပြန်လဲရန် မှာ) |

## 4. Practical — tools နှင့် commands

```bash
ipmitool sensor list          # sensor အားလုံး (temp, voltage, fan)
ipmitool chassis power cycle  # server reboot (cold-ish)
ipmitool chassis power status
```

- `sensor list` — BMC မှ မြင်ရသော physical state အပြည့် — fan RPM, inlet temp, voltage
- `chassis power status` — server ပိတ်/ဖွင့် အခြေအနေ ကြည့်ရန်
- `chassis power cycle` — OS မတက်တော့လျှင် remote reboot လုပ်ရာတွင် သုံးပါသည်
  (⚠️ ထို node ပေါ်က workload များ ပျက်သွားမည် — မလုပ်ခင် workload ရှိ/မရှိ စစ်ပါ)

```bash
dmidecode        # BIOS, motherboard, memory, CPU, serial numbers
```

စက် တပ်ဆင်ခြင်း/asset မှတ်တမ်းတင်ခြင်းတွင် အသုံးပြုပါသည် — "The Midnight Deployment"
mission ကဲ့သို့ **စက်အသစ် bring-up** တွင် ပထမဆုံး စစ်ဆေးရန် tool ဖြစ်ပါသည်။

### Bring-up / Deployment Sequence ⭐ (D1.1 — စာမေးပွဲ အမေးများ)

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

⚠️ **sequence က ကိုယ်တိုင် မေးခွန်း ဖြစ်တတ်ပါသည်** — "next step" / "must be true before"
ပုံစံဖြင့် မေးလေ့ ရှိပါသည်။ ဥပမာ — firmware update သည် ③ ဖြစ်ပြီး physical install နှင့်
BMC config ပြီးမှ ဆောင်ရွက်ရပါမည် (firmware ကို အဆင့် ① မှာ မလုပ်ပါနှင့်)။

## 5. လုပ်ငန်းသုံး သတိပေးချက်များ (Operational Cautions)

- **physical မလုပ်ခင် software remediation အဆင့် ကုန်အောင် စစ်ပါ** — reset မအောင်မြင်မှ
  reseat၊ ထပ်ပျက်မှ RMA ဆိုသော အစဉ်က ကုန်ကျစရိတ်နှင့် အချိန် လျှော့ချပေးပါသည်။
- `ipmitool chassis power cycle` ကဲ့သို့ power control command များသည် ထို node ပေါ်က
  workload အားလုံးကို သက်ရောက်ပါသည် — maintenance window နှင့် approval မရှိဘဲ
  မလုပ်ပါနှင့်။
- BMC သည် စက်တစ်ခုလုံးကို ထိန်းချုပ်နိုင်သောကြောင့် credentials ကို ဂရုတစိုက် စီမံပြီး
  OOB network ကို production network မှ ခွဲထားပါ (security အခြေခံ)။
- firmware update (③) သည် အမှားမခံနိုင်သော အဆင့် — version နှင့် platform
  ကိုက်ညီမှုကို vendor documentation ဖြင့် အတည်ပြုပြီးမှ ဆောင်ရွက်ပါ။

## 6. Self-Check

1. BMC နှင့် OOB ကွာခြားချက် ဘာလဲ။
   **အဖြေ** — BMC = server ပေါ်က သီးခြား controller (chip)၊ OOB = production network ပြင်ပ management network မှ စီမံခြင်း နည်းလမ်း ဖြစ်ပါသည်။
2. server OS မတက်တော့လျှင် remote reboot လုပ်သော command ဘာလဲ။
   **အဖြေ** — `ipmitool chassis power cycle` (workload ပျက်မည်ကို ကြိုတင် စဉ်းစားပြီးမှ)။
3. bring-up sequence တွင် firmware update က မည်သည့် အဆင့်လဲ။
   **အဖြေ** — ③ — physical install နှင့် BMC/OOB config ပြီးမှ ဆောင်ရွက်ရပါသည်။
4. NVLink link ဆက်မကောင်းလျှင် မည်သည့် physical action လုပ်ရမလဲ။
   **အဖြေ** — NVLink bridge ကို reseat လုပ်ပါ — ဆက်မကောင်းလျှင် replace/RMA စဉ်းစားပါ။

## 7. References

- DGX Administration: https://docs.nvidia.com/datacenter/dgx/
- BMC/IPMI: Intelligent Platform Management Interface spec
- NVIDIA HGX firmware: HGX user guide

## Technical review notes

- ပြင်ဆင်မှု အနေဖြင့် — `chassis power cycle` နှင့် firmware update တို့တွင် workload
  ထိခိုက်မှု/version ကိုက်ညီမှု သတိပေးချက်များ ဖြည့်စွက်ထားပြီး bring-up sequence တွင်
  firmware update အဆင့်ကို ဥပမာဖြင့် ရှင်းလင်းထားပါသည်။
- D1 (31%) ကဲ့သို့ စာမေးပွဲ domain အလေးပေးမှု ဂဏန်းများသည် မူရင်း course material
  အတိုင်း ထားခြင်းဖြစ်ပြီး စာမေးပွဲ blueprint ဗားရှင်းပေါ် မူတည်၍ ပြောင်းလဲနိုင်သဖြင့်
  **needs verification** ဖြစ်ပါသည်။
- ပြင်ဆင်ရန် အခြားအချက် မတွေ့ပါ။
