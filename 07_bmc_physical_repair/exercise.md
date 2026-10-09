# Module 07 — Exercise

## E1. Inventory ဖတ်ခြင်း
1. `dmidecode | head` ပုံစံ (simulator တွင် `dmidecode`) run ပြီး — CPU model, core count, RAM မှတ်တမ်းတင်ပါ
2. `ipmitool sensor list` run ပြီး — အပူချိန်, fan, voltage reading ၃ ခု ရွေး မှတ်ပါ

## E2. Midnight Deployment mission
1. Missions ထဲက **The Midnight Deployment** ကို run ပါ
2. mission story ထဲက requirement များ ဖတ်ပြီး —
   - ပထမဆုံး run ခဲ့သော command များ စာရင်း ပြုစုပါ
   - GPU 6 `ERR!` ကို မည်သို့ ဖြေရှင်းခဲ့သနည်း အဆင့်ဆင့် ရေးပါ
3. debrief ထဲက သင်ခန်းစာ များ မှတ်တမ်းတင်ပါ

## E3. Physical action ဆုံးဖြတ်ချက်
ဤအခြေအနေ ၃ ခုအတွက် မည်သည့် physical action ရွေးမလဲ —
1. GPU reset ၂ ကြိမ် လုပ်သော်လည်း XID 43 ထပ်တက်
2. NVLink link down ဖြစ်ပြီး fabric manager restart ဖြင့် မပြေလည်
3. ECC uncorrectable error ထပ်ခါတလဲ တက်နေ
