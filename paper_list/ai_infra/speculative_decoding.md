# Speculative Decoding

Papers and implementations for drafting future tokens cheaply and verifying them with a target model. This reduces sequential decode steps rather than sparsifying attention over existing context. Start with the [LLM foundations](../../README.md#llm); related systems topics are [inference and KV-cache management](acceleration.md#inference-kv-cache-and-serving) and [sparse attention kernels](sparse_attention.md).

Sources checked on **2026-10-16**. Paper dates use the first arXiv submission month, not code or model release dates. Runtime support and checkpoint formats are version-specific.

## Foundational Speculative Sampling

- (2022-11) **Fast Inference from Transformers via Speculative Decoding** [Paper](https://arxiv.org/abs/2211.17192) - Uses a cheaper draft model and parallel target verification to accelerate generation while preserving the target distribution under the algorithm's sampling assumptions.
- (2023-02) **Accelerating Large Language Model Decoding with Speculative Sampling** [Paper](https://arxiv.org/abs/2302.01318) - Develops modified rejection sampling for draft continuations, preserving the target distribution within hardware numerics.

Draft quality controls how much proposed work survives verification. A more accurate draft is not necessarily faster overall: drafting, target verification, and scheduling all contribute to the step cost.

## Autoregressive and Feature-conditioned Drafting

### EAGLE Family

- (2024-01) **EAGLE: Speculative Sampling Requires Rethinking Feature Uncertainty** [Paper](https://arxiv.org/abs/2401.15077) - Autoregressive drafting in target-model feature space, with token information to address feature uncertainty.
- (2024-06) **EAGLE-2: Faster Inference of Language Models with Dynamic Draft Trees** [Paper](https://arxiv.org/abs/2406.16858) - Adapts draft-tree structure to candidate confidence instead of using a fixed tree.
- (2025-03) **EAGLE-3: Scaling up Inference Acceleration of Large Language Models via Training-Time Test** [Paper](https://arxiv.org/abs/2503.01840) - Replaces feature prediction with direct token prediction conditioned on fused multi-layer target features, using training-time test to improve scaling.
- [Official code](https://github.com/SafeAILab/EAGLE) - Training and inference implementations for all three generations. Match the draft checkpoint to the target model, feature extraction scheme, tokenizer, and runtime format.

## Multi-head and Model-integrated Drafting

### Medusa

- (2024-01) **Medusa: Simple LLM Inference Acceleration Framework with Multiple Decoding Heads** [Paper](https://arxiv.org/abs/2401.10774) | [Official code](https://github.com/FasterDecoding/Medusa).
- **Method:** Adds heads for future-token predictions and verifies their candidate combinations with tree attention. Medusa-1 trains the added heads; Medusa-2 also updates the base model.
- **Correctness boundary:** The repository's typical acceptance scheme relaxes matching the original distribution in non-greedy generation. Do not label that mode as exact speculative rejection sampling or assume that every multiple-head decoder is lossless.

### Multi-token Prediction

- (2024-04) **Better & Faster Large Language Models via Multi-token Prediction** [Paper](https://arxiv.org/abs/2404.19737) - Trains multiple future-token heads over a shared trunk, improving learning and enabling faster inference.
- (2024-12) **DeepSeek-V3 Technical Report** [Paper](https://arxiv.org/abs/2412.19437v1) | [Official model repository](https://github.com/deepseek-ai/DeepSeek-V3) - A model-scale example of MTP modules used during training and for speculative generation.
- **Boundary:** MTP is a model-training/architecture mechanism, not a replacement for the verifier's acceptance algorithm. Serving requires the compatible checkpoint modules and runtime support; it is not available automatically for an arbitrary next-token model.

## Parallel and Semi-autoregressive Drafting

### DFlash

- (2026-02) **DFlash: Block Diffusion for Flash Speculative Decoding** [Paper](https://arxiv.org/abs/2602.06036) | [Official code](https://github.com/z-lab/dflash).
- **Method:** A lightweight block-diffusion draft model uses target context features to propose a whole block in one forward pass, avoiding a sequential autoregressive draft loop.
- **Boundary:** The target model still verifies the proposed tokens. A block-diffusion drafter does not turn the target LLM into a diffusion model. Use a draft checkpoint and backend explicitly supported by the chosen target and runtime.

### DSpark and DeepSpec

- (2026-07) **DSpark: Confidence-Scheduled Speculative Decoding with Semi-Autoregressive Generation** [Paper](https://arxiv.org/abs/2607.05147) | [Official DeepSpec code](https://github.com/deepseek-ai/DeepSpec).
- **Draft design:** Couples a block-parallel backbone with a lightweight sequential component to model dependencies within the block and reduce acceptance decay toward its suffix.
- **Serving design:** Predicts prefix-survival confidence and selects a per-request verification length using the serving engine's throughput profile and current load. Draft length and verify length are separate controls.
- **Training/evaluation:** DeepSpec provides data preparation, target-feature caching, and training/evaluation for DSpark, DFlash, and EAGLE-3. Its released checkpoints were trained against particular targets and data modes; domain or reasoning-mode changes can require draft adaptation.
- **Integration boundary:** DeepSpec's offline acceptance evaluation is not a reproduction of DeepSeek's production serving system. A DSpark checkpoint alone also does not establish that an engine enables the paper's calibrated, load-aware verification schedule.

## Draft Training Frameworks

- **SpecForge:** [Code](https://github.com/sgl-project/SpecForge) | (2026-03) [SpecForge: A Flexible and Efficient Open-Source Training Framework for Speculative Decoding](https://arxiv.org/abs/2603.18567). SGLang's framework for draft training, including EAGLE-3, DFlash, and DSpark. The reviewed version distinguishes online/offline target-feature production and colocated/disaggregated training topologies; check the supported method/topology matrix.
- **Speculators:** [Code](https://github.com/vllm-project/speculators) | [DSpark guide](https://docs.vllm.ai/projects/speculators/en/latest/user_guide/algorithms/dspark/). Provides draft training, evaluation, and reusable checkpoint formats for vLLM. Its DSpark implementation adds Markov and confidence heads to a DFlash-derived backbone; verify the supported target architecture and serving configuration.

## Serving Integration

- **SGLang:** [DSpark integration report](https://www.lmsys.org/blog/2026-07-06-dspark-sglang/) - Confidence calibration, per-request variable-length verification, compact/ragged batches, CUDA graphs, engine cost profiling, and overlap scheduling. The report separates the algorithm from the engineering needed to realize its latency/throughput trade-off.
- **vLLM:** [Speculative decoding documentation](https://docs.vllm.ai/en/latest/features/speculative_decoding/) - Available drafting methods, configuration, feature compatibility, and correctness tests. Read the losslessness section separately from reproducibility: floating-point and batching effects can still change outputs or log probabilities.
- **llama.cpp:** [Speculative decoding documentation](https://github.com/ggml-org/llama.cpp/blob/master/docs/speculative.md) - Local draft-model and model-integrated modes, with runtime-specific checkpoint and backend requirements. Do not assume a mode supported by one engine is available in every other backend.

## Evaluation and Correctness

- Report target and draft model revisions, training data/mode, tokenizer compatibility, hardware, precision/quantization, parallelism, prompt/output lengths, and sampling settings.
- Define acceptance rate and accepted tokens per verification step explicitly, including whether the extra target-generated token is counted. Compare these with actual wall-clock latency, not as substitutes for it.
- Measure drafting, feature transfer, confidence calibration/scheduling, target verification, and KV-cache commit/rollback costs. A wider draft or verification tree can increase work even when acceptance improves.
- Sweep concurrency and report aggregate throughput alongside per-user inter-token latency or decode speed. Small-batch speedups do not establish a high-load serving benefit.
- Distinguish greedy token equality, distribution-preserving rejection sampling, and approximate/typical acceptance. Distributional correctness does not imply byte-for-byte equality for a seeded stochastic run across different execution paths.

[Back to Speculative Decoding](../../README.md#speculative-decoding) | [Back to AI Infra](../../README.md#ai-infra)
