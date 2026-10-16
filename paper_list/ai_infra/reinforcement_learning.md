# RL Training Systems

Infrastructure for coordinating rollout generation, reward computation, and policy training. Start with the [LLM reinforcement learning guide](../llm/reinforcement_learning.md) for PPO, GRPO, preference optimization, and agent-training algorithms.

## Training Systems

- (2024-05) **OpenRLHF: An Easy-to-use, Scalable and High-performance RLHF Framework** [paper](https://arxiv.org/abs/2405.11143) [code](https://github.com/OpenRLHF/OpenRLHF) - Provides a distributed alignment framework combining Ray, vLLM, DeepSpeed, and Hugging Face Transformers to coordinate RL training and generation.
- (2024-09) **HybridFlow: A Flexible and Efficient RLHF Framework** [paper](https://arxiv.org/abs/2409.19256) [code](https://github.com/verl-project/verl) - Combines single-controller and multi-controller execution with efficient model resharding between generation and training; its implementation is released as verl.

## Implementation Resources

- [RL and post-training frameworks](../../README.md#reinforcement-learning-and-post-training)
- [Training and inference systems](acceleration.md)
- [Inference engines](../../README.md#llm-inference)
- [Cluster orchestration](../../README.md#cluster-orchestration-and-deployment)

[Back to AI Infra](../../README.md#ai-infra)
