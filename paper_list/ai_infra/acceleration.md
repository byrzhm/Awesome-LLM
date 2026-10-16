# Training and Inference Systems
> AI infrastructure papers on training parallelism, memory efficiency, GPU kernels, and inference systems.

This list focuses on systems contributions and the bottlenecks they address. Dates use the first arXiv submission month. For rollout scheduling and distributed post-training, see [RL training systems](reinforcement_learning.md#training-systems).

## Distributed Training and Memory Optimization

- (2018-11) **GPipe: Efficient Training of Giant Neural Networks using Pipeline Parallelism** [Paper](https://arxiv.org/abs/1811.06965) - Uses microbatching and pipeline parallelism to train models that span multiple accelerators.
- (2019-09) **Megatron-LM: Training Multi-Billion Parameter Language Models Using Model Parallelism** [Paper](https://arxiv.org/abs/1909.08053) | [Code](https://github.com/NVIDIA/Megatron-LM) - Introduces tensor model parallelism for Transformer layers to distribute large-model training across GPUs.
- (2019-10) **ZeRO: Memory Optimizations Toward Training Trillion Parameter Models** [Paper](https://arxiv.org/abs/1910.02054) | [Code](https://github.com/microsoft/DeepSpeed) - Partitions optimizer states, gradients, and parameters across data-parallel workers to reduce redundant memory use.
- (2022-01) **Using Deep and Megatron to Train Megatron-Turing NLG 530B, A Large-Scale Generative Language Model** [Paper](https://arxiv.org/abs/2201.11990) - Combines Megatron tensor parallelism with DeepSpeed pipeline parallelism and memory optimization for large-scale training.

## Attention and GPU Kernels

- (2022-05) **FlashAttention: Fast and Memory-Efficient Exact Attention with IO-Awareness** [Paper](https://arxiv.org/abs/2205.14135) | [Code](https://github.com/Dao-AILab/flash-attention) - Uses tiling and recomputation to reduce transfers between GPU high-bandwidth memory and on-chip SRAM.
- (2023-07) **FlashAttention-2: Faster Attention with Better Parallelism and Work Partitioning** [Paper](https://arxiv.org/abs/2307.08691) | [Code](https://github.com/Dao-AILab/flash-attention) - Improves GPU work partitioning and reduces non-matmul work in exact attention.

## Inference, KV Cache, and Serving

- (2023-03) **FlexGen: High-Throughput Generative Inference of Large Language Models with a Single GPU** Ying Sheng et al. [Paper](https://arxiv.org/abs/2303.06865) | [Code](https://github.com/FMInference/FlexGen) - Schedules computation and offloads weights, activations, and KV cache across GPU, CPU, and disk for throughput-oriented inference.
- (2023-09) **Efficient Memory Management for Large Language Model Serving with PagedAttention** [Paper](https://arxiv.org/abs/2309.06180) | [Code](https://github.com/vllm-project/vllm) - Manages KV cache in blocks to reduce fragmentation and enable memory sharing across requests.
- (2024-01) **DistServe: Disaggregating Prefill and Decoding for Goodput-optimized Large Language Model Serving** [Paper](https://arxiv.org/abs/2401.09670) - Separates prefill and decoding to reduce interference and optimize throughput under latency objectives.
- (2024-03) **Taming Throughput-Latency Tradeoff in LLM Inference with Sarathi-Serve** [Paper](https://arxiv.org/abs/2403.02310) - Uses chunked prefills and stall-free scheduling to balance serving throughput and decode latency.

## Useful Resources

- [Training frameworks](../../README.md#llm-training-frameworks)
- [Inference engines](../../README.md#llm-inference)
- [GPU kernels and compilers](../../README.md#gpu-kernels-and-compilers)
- [Distributed communication](../../README.md#distributed-communication)
- [Profiling, benchmarks, and observability](../../README.md#profiling-benchmarks-and-observability)

[Back to AI Infra](../../README.md#ai-infra)
