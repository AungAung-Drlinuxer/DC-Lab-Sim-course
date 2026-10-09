# Module 06 — Solution

## E1
- A100: NVLink 3rd gen, 12 links — H100: 4th gen, 18 links (bandwidth ပိုမြင့်)
- topo -m: DGX တွင် GPU အားလုံး NVLink ချိတ်ဆက် (NV12/NV18)

## E2
- NVLink Down inject ပြီး: link အချို့ Inactive/0 MB/s ဖြစ်သွား
- error counter တက်လာနိုင်
- fabric manager restart + reseat bridge → link ပြန် Active
- ဆက်မကောင်းပါက bridge/cable replace

## E3
- ibstat: State Active, Rate HDR 200Gb/s (A100) / NDR 400Gb/s (H100)
- Dashboard က visual fabric map — terminal က port-level detail
