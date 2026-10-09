# Module 02 — Solution

## E1
- discovery: GPU 0-7 (8 လုံး) + အသုံးပြုနေသော process များ
- 1001 = GPU util, 1002 = mem util, 1004 = PCIe TX bandwidth
- Training ဖြင့်: 1001 ~95%, 1002 မြင့်တက်, PCIe traffic တက်

## E2
- `-r 1` quick: အချိန်တို၊ state မြန်စစ်
- ECC inject ပြီး: diag က error/warning ပြရမည် (health watch က ဖမ်းမိ)
- `dcgmi health -s`: watch enable ဖြစ်မှု အခြေအနေ

## E3
- `nvsm show health`: system အဆင့် OK/Degraded/Critical အနှစ်ချုပ်
- XID inject ပြီးနောက် health ကျဆင်းကြောင်း ပြရမည်
