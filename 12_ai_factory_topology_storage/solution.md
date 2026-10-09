# Module 12 — Solution

## E1
1. Compute = GPU node ချင်း E/W (NCCL)၊ Storage = node↔storage N/S (parallel FS)
2. NCCL traffic ကို path တစ်ခုတည်း မကျစေ — hotspot ကာကွယ်ပြီး bandwidth ညီမျှ

## E2
1. rack အတွင်း 1m — **DAC** (ဈေးသက်သာ)
2. 200m — **optical transceiver + fiber** (AOC က အလုံးလိုက် မလဲနိုင်/အကွာ မလုံလောက်)

## E3
- storage read throughput (benchmark) တိုင်းတာ
- network path (storage fabric) error/latency
- dataloader config (parallelism, prefetch) နှင့် file system mount option
