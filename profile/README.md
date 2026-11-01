<p align="center"><img src="banner.png" alt="What are Numbers?" width="100%"></p>

NukeTorch is an AI engine for Apple silicon: training and inference of large models on Macs, from one Mac to several joined by Thunderbolt.

## Pipeline parallelism

Our pipeline parallelism spreads one model across many machines connected by a single cable in between. We used it for Qwen3.5-9B LoRA fine-tuning and Qwen3.8 Flash Next inference; the fine-tuning reached 93.0% pipeline efficiency against four single machines.

## Results

| Benchmark | Performance | Compared with |
|---|---:|---|
| **Training:** Qwen3.5 9B fine-tuning, Apple WWDC26 recipe, four MacBook Pro M5 Max 48 GB | **5,358.5** tokens/s | over 8× Apple's WWDC26 figure of about 600 tokens/s (four M3 Ultra Mac Studios) |
| **Training:** Qwen3.5 9B fine-tuning, Apple WWDC26 recipe, one MacBook Pro M5 Max 48 GB | **1,440.8** tokens/s | 8× Apple's one-Mac figure of about 180 tokens/s (one M3 Ultra) |
| **Robotics:** GR00T N1.7, four M5 Max, 192 concurrent policies | **152.07** chunks/s | 2.45× the fastest published H100 result (62.2 chunks/s)¹ |
| **Robotics:** GR00T N1.7, one MacBook Pro M5 Max, one request at a time | **22.6** chunks/s (44.34 ms per call) | 0.63× NVIDIA's own H100 figure (35.9 chunks/s); one MacBook Pro M5 Max lists at $4,999, one H100 card sells for $25,000 or more² |
| **Inference:** Qwen3.8 Flash Next pipelined over four MacBook Pro M5 Max, one user | **177.7** tokens/s | 1.18× the published one-H100 NVL result, ~4-bit Unsloth GGUF (151 tokens/s)³ |
| **Inference:** Qwen3.8 Flash Next pipelined over four MacBook Pro M5 Max, 32 users | **859.0** tokens/s combined | No published H100 result for 32 users at once found. |

¹ The H100 figures are published results timed one request at a time; no H100 rate with many requests at once is published. Ours: 48 clients per Mac, FP16; a longer run measured 136.83 chunks/s.
² Ours: one request at a time; the test pooled 200 timed calls across 10 fresh processes, with 100 warm-up calls excluded; camera preparation and action decoding are included. NVIDIA's: its TensorRT pipeline on an H100 80 GB, per its hardware recommendation guide.
³ These are different machines, packs and prompts, so this is a reference, not a same-conditions comparison; the published figures were not run here. 177.7 and 859.0 are chat-app takes; the fixed-protocol rates (133.0 and 553.1 tokens/s) are in the report.

Methods, conditions and every run: https://nuketorch.ai/benchmarks

Evaluation release 1.0.0: https://github.com/NukeTorch/nuketorch · contact@nuketorch.ai
