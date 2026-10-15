# Reinforcement Learning for Large Language Models

A curated reading list covering RLHF/RLAIF, reinforcement learning with verifiable rewards (RLVR), agent training, and training systems. Sources checked on **2026-10-15**; this is not an exhaustive bibliography. Dates denote the **first arXiv submission**, and titles follow the linked revisions. DPO is included as an explicitly labeled RL-free comparison.

See also the [RLHF starting points](RLHF.md), [alignment papers](alignment.md), and [frontier model reports](frontier_models.md) for model-scale RL case studies including MiMo-V2.6, Kimi K3, and DeepSeek V4.

## Suggested Reading Path

1. **Preference alignment:** PPO → InstructGPT → Constitutional AI → DPO and RLOO, comparing learned rewards, AI feedback, and direct preference losses.
2. **Verifiable reasoning:** DeepSeekMath/GRPO → DeepSeek-R1 → DAPO and Dr. GRPO → GSPO → LUSPO, tracking changes in baselines, clipping, and length normalization.
3. **Interactive agents:** Search-R1 → Agent Lightning → Single-Rollout Asynchronous Optimization → Agent Lightning v1.0, moving from search to asynchronous training with real agent harnesses.
4. **Evaluate the claims:** Read the capability-boundary study alongside ProRL, then examine spurious rewards and memorization; compare base models, sampling budgets, and training duration.

## Foundations, RLHF, and Preference Optimization

- (2017-07) **Proximal Policy Optimization Algorithms** [paper](https://arxiv.org/abs/1707.06347) — Introduces PPO's surrogate policy objectives, enabling repeated minibatch updates on sampled experience with a practical balance of simplicity and stability.
- (2022-03) **Training language models to follow instructions with human feedback** [paper](https://arxiv.org/abs/2203.02155) — Develops InstructGPT through supervised demonstrations, human preference modeling, and reinforcement learning to improve instruction following.
- (2022-12) **Constitutional AI: Harmlessness from AI Feedback** [paper](https://arxiv.org/abs/2212.08073) — Combines principle-guided self-critique with reinforcement learning from AI-generated preferences, reducing reliance on human harmlessness annotations.
- (2023-05) **Direct Preference Optimization: Your Language Model is Secretly a Reward Model** [paper](https://arxiv.org/abs/2305.18290) — **RL-free comparison:** optimizes a classification loss over preference pairs without an explicit reward model or online policy sampling during fine-tuning.
- (2024-02) **Back to Basics: Revisiting REINFORCE Style Optimization for Learning from Human Feedback in LLMs** [paper](https://arxiv.org/abs/2402.14740) — Revisits sequence-level REINFORCE and leave-one-out baselines (RLOO) as simpler, critic-free alternatives to PPO for preference alignment.
- (2025-01) **REINFORCE++: Stabilizing Critic-Free Policy Optimization with Global Advantage Normalization** [paper](https://arxiv.org/abs/2501.03262) — Normalizes advantages across the global batch and studies critic-free variants for general preference alignment and group-sampled reasoning.

## Verifiable Rewards and Reasoning

- (2024-02) **DeepSeekMath: Pushing the Limits of Mathematical Reasoning in Open Language Models** [paper](https://arxiv.org/abs/2402.03300) [project](https://github.com/deepseek-ai/DeepSeek-Math) — Introduces Group Relative Policy Optimization (GRPO), replacing a learned critic with group-derived baselines to reduce RL training resources.
- (2025-01) **DeepSeek-R1: Incentivizing Reasoning Capability in LLMs via Reinforcement Learning** [paper](https://arxiv.org/abs/2501.12948) [project](https://github.com/deepseek-ai/DeepSeek-R1) — Studies reasoning through large-scale RL in R1-Zero, then combines cold-start data and multiple training stages in R1 and distills its outputs.
- (2025-03) **DAPO: An Open-Source LLM Reinforcement Learning System at Scale** [paper](https://arxiv.org/abs/2503.14476) [code](https://github.com/BytedTsinghua-SIA/DAPO) — Presents decoupled clipping, dynamic sampling, and an open training recipe and dataset for reproducing large-scale reasoning RL.
- (2025-03) **Understanding R1-Zero-Like Training: A Critical Perspective** [paper](https://arxiv.org/abs/2503.20783) [code](https://github.com/sail-sg/understand-r1-zero) — Examines base-model effects and GRPO optimization bias, introducing Dr. GRPO to improve token efficiency while maintaining reasoning performance.
- (2025-03) **Open-Reasoner-Zero: An Open Source Approach to Scaling Up Reinforcement Learning on the Base Model** [paper](https://arxiv.org/abs/2503.24290) [code](https://github.com/Open-Reasoner-Zero/Open-Reasoner-Zero) — Studies a reproducible reasoning recipe using vanilla PPO, a learned critic, rule-based rewards, and no KL regularization.
- (2025-05) **ProRL: Prolonged Reinforcement Learning Expands Reasoning Boundaries in Large Language Models** [paper](https://arxiv.org/abs/2505.24864) — Reports expanded solution coverage under prolonged RL with KL control, reference-policy resets, and diverse tasks, complementing capability-boundary critiques.
- (2025-07) **Group Sequence Policy Optimization** [paper](https://arxiv.org/abs/2507.18071) — Moves importance ratios and clipping to the sequence level, reporting improved training efficiency and greater stability for MoE reasoning models.
- (2026-02) **Length-Unbiased Sequence Policy Optimization: Revealing and Controlling Response Length Variation in RLVR** [paper](https://arxiv.org/abs/2602.05261) — Analyzes response-length dynamics and introduces LUSPO to address GSPO length bias and response-length collapse in mathematical and multimodal reasoning experiments.

## Agents, Search, and Tool Use

- (2025-03) **Search-R1: Training LLMs to Reason and Leverage Search Engines with Reinforcement Learning** [paper](https://arxiv.org/abs/2503.09516) [code](https://github.com/PeterGriffinJin/Search-R1) — Trains interleaved reasoning and multi-turn search using outcome rewards and masks retrieved tokens during policy optimization.
- (2025-08) **Agent Lightning: Train ANY AI Agents with Reinforcement Learning** [paper](https://arxiv.org/abs/2508.03680) [code](https://github.com/microsoft/agent-lightning) — Separates agent execution from RL training and introduces trajectory decomposition and credit assignment for existing agent workflows.
- (2026-07) **Single-Rollout Asynchronous Optimization for Agentic Reinforcement Learning** [paper](https://arxiv.org/abs/2607.07508) — Uses one rollout per prompt, value-model training, and strict two-sided token clipping to address instability and off-policy effects in asynchronous agent RL.
- (2026-08) **Agent Lightning v1.0: Towards Harnessed Agentic RL** [paper](https://arxiv.org/abs/2608.17528) [code](https://github.com/microsoft/agent-lightning) — Studies training with deployment-time agent harnesses, including retokenization, sample merging, advantage calculation, and a reproducible coding-agent training pipeline.

## Training Systems

- (2024-05) **OpenRLHF: An Easy-to-use, Scalable and High-performance RLHF Framework** [paper](https://arxiv.org/abs/2405.11143) [code](https://github.com/OpenRLHF/OpenRLHF) — Provides a distributed alignment framework combining Ray, vLLM, DeepSpeed, and Hugging Face Transformers to coordinate RL training and generation.
- (2024-09) **HybridFlow: A Flexible and Efficient RLHF Framework** [paper](https://arxiv.org/abs/2409.19256) [code](https://github.com/verl-project/verl) — Combines single-controller and multi-controller execution with efficient model resharding between generation and training; its implementation is released as verl.

## Analysis, Limitations, and Surveys

- (2025-04) **Does Reinforcement Learning Really Incentivize Reasoning Capacity in LLMs Beyond the Base Model?** [paper](https://arxiv.org/abs/2504.13837) — Uses large-budget pass@k evaluations to argue that the studied RLVR setups improve sampling efficiency without expanding the base model's reasoning coverage.
- (2025-06) **Spurious Rewards: Rethinking Training Signals in RLVR** [paper](https://arxiv.org/abs/2506.10947) — Reports reasoning gains from uninformative rewards in some Qwen models and studies clipping bias, emphasizing validation across different model families.
- (2026-01) **Spurious Rewards Paradox: Mechanistically Understanding How RLVR Activates Memorization Shortcuts in LLMs** [paper](https://arxiv.org/abs/2601.11061) — Uses circuit analysis and causal interventions to investigate how spurious-reward training can activate memorization shortcuts in the studied models.
- (2026-04) **A Survey of Reinforcement Learning for Large Language Models under Data Scarcity: Challenges and Solutions** [paper](https://arxiv.org/abs/2604.17312) — Organizes data-efficient RL methods around data, training, and framework perspectives, covering scarce external supervision and limited generated experience.

Interpret empirical results within each paper's model, reward, data, and compute setting; these studies do not establish a universal answer to whether RL creates new reasoning capabilities.
