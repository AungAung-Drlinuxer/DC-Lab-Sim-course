# Module 07 — Solution

## E1
- dmidecode: 2× AMD EPYC 7742 (128 cores), RAM 1024GB — dashboard နှင့် ကိုက်ညီရမည်
- ipmitool: inlet temp, fan RPM, 12V rail ကဲ့သို့ sensor များ ပုံမှန် range အတွင်း

## E2
- နမူနာ sequence: dmidecode (inventory) → ipmitool (BMC/sensors) → nvidia-smi (GPU state) → ပြဿနာ တွေ့ (GPU 6 ERR!) → document → reset → cluster validate
- ဖြေရှင်းပုံ: error ဖမ်း → အကြောင်းရင်း သေချာ → software reset → မရပါက reseat/replace → `dcgmi diag` ဖြင့် validate

## E3
1. **Reseat GPU** (software ကုန်ပြီ — physical အဆင့်)
2. **Reseat NVLink bridge**
3. **Mark for RMA** (ထပ်တက်နေသော uncorrectable ECC = hardware replace)
