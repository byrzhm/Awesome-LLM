# Mixture-of-Experts Models

Model architecture and sparse scaling prerequisites for expert-parallel training and serving. These papers are also part of the [milestone reading list](../../README.md#milestone-papers).

## Architecture and Scaling

- (2021-01) [Switch Transformers: Scaling to Trillion Parameter Models with Simple and Efficient Sparsity](https://arxiv.org/abs/2101.03961) - Sparse expert routing and scaling with a simple top-1 router.
- (2021-12) [GLaM: Efficient Scaling of Language Models with Mixture-of-Experts](https://arxiv.org/abs/2112.06905) - Sparse conditional computation for language modeling.
- (2024-05) [DeepSeek-V2: A Strong, Economical, and Efficient Mixture-of-Experts Language Model](https://arxiv.org/abs/2405.04434) - DeepSeekMoE and Multi-head Latent Attention as a model-level basis for efficient training and inference.
- (2024-09) [OLMoE: Open Mixture-of-Experts Language Models](https://arxiv.org/abs/2409.02060) - An open MoE research model with training and analysis resources.
- (2024-12) [DeepSeek-V3 Technical Report](https://arxiv.org/abs/2412.19437v1) - Large-scale MoE design, load balancing, and multi-token prediction.
- (2025-07) [Kimi K2: Open Agentic Intelligence](https://arxiv.org/abs/2507.20534) - A trillion-parameter MoE model and its training recipe.

## Continue with AI Infra

- [Distributed training](../../README.md#llm-training-frameworks) - Tensor, pipeline, data, and expert parallelism.
- [Distributed communication](../../README.md#distributed-communication) - Collective operations and expert dispatch/combine.
- [Inference and serving](../../README.md#llm-inference) - Serving sparse models under memory and latency constraints.

[Back to LLM](../../README.md#llm)
