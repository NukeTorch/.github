<p align="center"><img src="banner.png" alt="What are Numbers?" width="100%"></p>

NukeTorch is an AI engine for Apple silicon: training and inference of large models on Macs, from one Mac to several joined by Thunderbolt.

## NukeTorch 1.0.5: free, on up to 250 machines, each working alone

A fast inference server with an OpenAI-compatible API. Download it and repeat our numbers with the benchmark built into the program.

```sh
curl -fsSL https://github.com/NukeTorch/nuketorch/releases/download/1.0.5/install.sh | sh
```

| Model | One MacBook Pro M5 Max, 48 GB |
|---|---:|
| Qwen3.5-9B LoRA fine-tuning, 1,000 steps of the recipe Apple showed in WWDC26 session 233 | **1,242** trained tokens/s |
| Qwen3.6 35B-A3B 4-bit, with its DFlash2 draft model | **517.5** tokens/s (prompt A, 5 coding prompts, benchmark report) |
| Qwen3.8 27B 4-bit, with its DFlash2 draft model | **200.3** tokens/s (prompt A, 5 coding prompts, benchmark report) |
| NVIDIA GR00T N1.7 robot policy, 16 clients at once | **37.2** chunks/s (**44.8** ms for one request) |
| Nubotics robot policy (built on NVIDIA's GR00T N1.7 weights), one robot | **53.2** chunks/s (**18.81** ms a call; NVIDIA's H100 figure: 22.9 ms) |
| Whisper large-v3-turbo, speech to text | **2.97%** word error rate on LibriSpeech at **89** × real time |

How we measured, what to expect and how to repeat it: https://github.com/NukeTorch/nuketorch · Models: https://huggingface.co/NukeTorch

Free for personal use, schools and universities, and businesses and other organizations (not government entities or international organizations) that with their affiliates have less than US$10 million in revenue and fewer than 250 people, on up to 250 machines, each working alone; everyone else may evaluate it for 30 days, then needs a commercial license (contact@nuketorch.ai); terms per version in its LICENSE.md: https://github.com/NukeTorch/nuketorch/blob/1.0.5/LICENSE.md

## Pipeline parallelism: NukeTorch Enterprise, Multi Machines

Our pipeline parallelism spreads one model across many machines connected by a single cable in between. We used it for Qwen3.5-9B LoRA fine-tuning and inference of a larger model; the fine-tuning reached 93.0% pipeline efficiency against four single machines.

## Results

| Benchmark | Performance | Compared with |
|---|---:|---|
| **Training:** Qwen3.5 9B fine-tuning, Apple WWDC26 recipe, four MacBook Pro M5 Max 48 GB | **5,358.5** tokens/s | over 8× Apple's WWDC26 figure of about 600 tokens/s (four M3 Ultra Mac Studios) |
| **Training:** Qwen3.5 9B fine-tuning, Apple WWDC26 recipe, one MacBook Pro M5 Max 48 GB | **1,440.8** tokens/s | 8× Apple's one-Mac figure of about 180 tokens/s (one M3 Ultra) |
| **Robotics:** Nubotics 1.7B, one MacBook Pro M5 Max, one robot at a time | **53.2** chunks/s (18.81 ms per call) | 1.22× NVIDIA's own H100 figure for GR00T N1.7 on LIBERO-10 (43.7 chunks/s, 22.9 ms, one request at a time) |
| **Robotics:** GR00T N1.7, one MacBook Pro M5 Max, one request at a time | **22.6** chunks/s (44.34 ms per call) | 0.63× NVIDIA's own H100 figure (35.9 chunks/s)² |
| **Robotics:** GR00T N1.7, four M5 Max, 192 concurrent policies | **152.07** chunks/s | 2.45× the fastest published H100 result (62.2 chunks/s), which is for one request at a time¹ |
| **Inference:** a larger model pipelined over four MacBook Pro M5 Max, one user | **177.7** tokens/s | 1.18× the published one-H100 NVL result, ~4-bit Unsloth GGUF (151 tokens/s)³ |
| **Inference:** a larger model pipelined over four MacBook Pro M5 Max, 32 users | **859.0** tokens/s combined | No published H100 result for 32 users at once found. |

¹ The H100 figures are published results timed one request at a time; no H100 rate with many requests at once is published. Ours: 48 clients per Mac, FP16; a longer run measured 136.83 chunks/s.
² Ours: one request at a time; the test pooled 200 timed calls across 10 fresh processes, with 100 warm-up calls excluded; camera preparation and action decoding are included. NVIDIA's: its TensorRT pipeline on an H100 80 GB, per its hardware recommendation guide.
³ These are different machines, packs and prompts, so this is a reference, not a same-conditions comparison; the published figures were not run here. 177.7 and 859.0 are chat-app takes; the fixed-protocol rates (133.0 and 553.1 tokens/s) are in the report.

Methods, conditions and every run: https://nuketorch.ai/benchmarks

NukeTorch Enterprise P1.0.0 (Multi Machines) is by invitation. Ask for an evaluation license:

[![Request a NukeTorch Enterprise evaluation license](https://img.shields.io/badge/NukeTorch%20Enterprise-Request%20an%20evaluation%20license-121F47?style=for-the-badge)](mailto:contact@nuketorch.ai?subject=NukeTorch%20Enterprise%20evaluation%20request&body=Name%3A%0AOrganization%3A%0AMacs%20%28how%20many%2C%20which%20chips%2C%20memory%29%3A%0AModel%20or%20workload%3A%0A)

or write to contact@nuketorch.ai.
