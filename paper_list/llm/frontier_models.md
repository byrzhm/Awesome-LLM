# Recent Frontier Model Reports

Selected public papers and technical reports from Xiaomi MiMo, Moonshot AI Kimi, and DeepSeek, with a separate entry for OpenAI's GPT-6 Astra documentation. Sources checked on **2026-10-15**. See also [Reinforcement Learning for LLMs](reinforcement_learning.md).

Dates use the first submission shown in each paper's arXiv submission history, rather than the model launch date. DeepSeek-V4.1-Flash uses its dated official release, as marked below. Entries are chronological within each family; descriptions summarize the authors' reported contributions.

## Xiaomi MiMo

| Date | Model / topic | Paper | Main contribution | Official resources |
|:---:|---|---|---|---|
| 2025-05 | MiMo | [MiMo: Unlocking the Reasoning Potential of Language Model -- From Pretraining to Posttraining](https://arxiv.org/abs/2505.07608) | Reasoning-oriented pretraining with multi-token prediction, followed by RL on verifiable mathematics and programming tasks. | [Code and models](https://github.com/XiaomiMiMo/MiMo) |
| 2025-06 | MiMo-VL | [MiMo-VL Technical Report](https://arxiv.org/abs/2506.03569) | Four-stage multimodal pretraining and Mixed On-policy Reinforcement Learning (MORL) across different reward signals. | [Code and models](https://github.com/XiaomiMiMo/MiMo-VL) |
| 2025-12 | MiMo-Audio | [MiMo-Audio: Audio Language Models are Few-Shot Learners](https://arxiv.org/abs/2512.23808) | Large-scale audio-language pretraining for few-shot generalization, followed by reasoning-aware audio post-training. | [Code and models](https://github.com/XiaomiMiMo/MiMo-Audio) |
| 2026-01 | MiMo-V2-Flash | [MiMo-V2-Flash Technical Report](https://arxiv.org/abs/2601.02780) | A 309B-total/15B-active MoE combining hybrid attention, agentic RL, multi-teacher on-policy distillation, and speculative decoding. | [Code and models](https://github.com/XiaomiMiMo/MiMo-V2-Flash) |
| 2026-10 | MiMo-V2.6 | [MiMo-V2.6: Scaling Reinforcement Learning Towards Self-Improvement](https://arxiv.org/abs/2610.11959) | Scales asynchronous RL through larger batches, diverse agent environments, and groupwise agentic grading; releases training dynamics and infrastructure. | [Pro-RL weights](https://huggingface.co/XiaomiMiMo/MiMo-V2.6-Pro-RL), [Flash-RL weights](https://huggingface.co/XiaomiMiMo/MiMo-V2.6-Flash-RL), [Training logs](https://mimo.xiaomi.com/rl/mimo-v26/) |

The MiMo-7B model launched before its May 2025 paper; MiMo-V2-Flash launched in December 2025 before its January 2026 arXiv report.

## Moonshot AI Kimi

| Date | Model / topic | Paper | Main contribution | Official resources |
|:---:|---|---|---|---|
| 2025-01 | Kimi k1.5 | [Kimi k1.5: Scaling Reinforcement Learning with LLMs](https://arxiv.org/abs/2501.12599) | Long-context RL, partial rollouts, and long-to-short reasoning transfer to improve reasoning across inference budgets. | [Project](https://github.com/MoonshotAI/Kimi-k1.5) |
| 2025-04 | Kimi-VL | [Kimi-VL Technical Report](https://arxiv.org/abs/2504.07491) | Efficient MoE vision-language models with MoonViT and a long-thinking RL variant. | [Code and models](https://github.com/MoonshotAI/Kimi-VL) |
| 2025-07 | Kimi K2 | [Kimi K2: Open Agentic Intelligence](https://arxiv.org/abs/2507.20534) | A 1T-total/32B-active MoE trained with MuonClip, synthetic agentic data, and general RL for tool use. | [Code and models](https://github.com/MoonshotAI/Kimi-K2) |
| 2025-10 | Kimi Linear | [Kimi Linear: An Expressive, Efficient Attention Architecture](https://arxiv.org/abs/2510.26692) | Combines Kimi Delta Attention with Multi-head Latent Attention for efficient long-context processing and RL scaling. | [Code and models](https://github.com/MoonshotAI/Kimi-Linear) |
| 2026-02 | Kimi K2.5 | [Kimi K2.5: Visual Agentic Intelligence](https://arxiv.org/abs/2602.02276) | Joint text-vision pretraining and RL, with Agent Swarm for dynamically parallelized agent tasks. | [Code and models](https://github.com/MoonshotAI/Kimi-K2.5) |
| 2026-07 | Kimi K3 | [Kimi K3: Open Frontier Intelligence](https://arxiv.org/abs/2607.24653) | A 2.8T-total/104B-active MoE combining KDA, Attention Residuals, Stable LatentMoE, million-token context, and agentic RL. | [Code](https://github.com/MoonshotAI/Kimi-K3), [Weights](https://huggingface.co/moonshotai/Kimi-K3) |

Kimi K2.5 launched on January 27, 2026; its paper was first submitted on February 2, 2026.

## DeepSeek

| Date | Model / topic | Paper / technical report | Main contribution | Official resources |
|:---:|---|---|---|---|
| 2025-12 | DeepSeek-V3.2 | [DeepSeek-V3.2: Pushing the Frontier of Open Large Language Models](https://arxiv.org/abs/2512.02556) | DeepSeek Sparse Attention, scalable RL, and large-scale agent-task synthesis for reasoning and tool use. | [Weights and model card](https://huggingface.co/deepseek-ai/DeepSeek-V3.2) |
| 2025-12 | mHC | [mHC: Manifold-Constrained Hyper-Connections](https://arxiv.org/abs/2512.24880) | Constrains residual connection mappings to a manifold to improve training stability at scale. | — |
| 2026-01 | Engram | [Conditional Memory via Scalable Lookup: A New Axis of Sparsity for Large Language Models](https://arxiv.org/abs/2601.07372) | Adds conditional memory through constant-time lookup, complementing the conditional computation of MoE models. | — |
| 2026-04 | DeepSeek-V4 | [DeepSeek-V4: Towards Highly Efficient Million-Token Context Intelligence](https://arxiv.org/abs/2606.19348) | Hybrid compressed attention for million-token context; combines domain-expert RL with on-policy distillation. | [V4-Pro weights and report](https://huggingface.co/deepseek-ai/DeepSeek-V4-Pro) |
| 2026-09 (official release) | DeepSeek-V4.1-Flash | [DeepSeek-V4.1-Flash: Pushing the Limits of KV Cache Compression](https://huggingface.co/deepseek-ai/DeepSeek-V4.1-Flash/blob/main/DeepSeek_V41_Tech_Report.pdf) | Multimodal MoE with Causal Encoder-Decoder, CSA2, FP4 KV caching, and Engram; post-training includes RL and on-policy distillation. | [Weights and model card](https://huggingface.co/deepseek-ai/DeepSeek-V4.1-Flash), [Release announcement](https://api-docs.deepseek.com/news/news260910/) |

DeepSeek-V4 launched on April 24, 2026. The linked arXiv page lists April 26, 2026 in its submission history. DeepSeek-V4.1-Flash's date above is the September 10, 2026 official release date, not an arXiv submission date.

## OpenAI GPT-6 Astra: Official Documentation

| Date | Model | Public source | Scope |
|:---:|---|---|---|
| 2026-09-03 (API release) | GPT-6 Astra (`gpt-6-astra`) | [Official model documentation](https://developers.openai.com/api/docs/models/gpt-6-astra), [API changelog](https://developers.openai.com/api/docs/changelog) | Official documentation for complex reasoning, coding, computer use, and multi-step tool workflows. |

**GPT-6 Astra** is the official model name. As of the source check on October 15, 2026, the official OpenAI documentation reviewed here did not provide a separate public paper or technical-report link. This entry links model documentation and its release record.

[Back to LLM](../../README.md#llm)
