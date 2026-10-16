# Sparse Attention Kernels

Sparse attention implementations grouped by the work they skip: selected KV blocks, selected tokens, or blocks excluded by a supplied mask. Start with the [LLM foundations](../../README.md#llm) and [dense attention kernel papers](acceleration.md#attention-and-gpu-kernels). Sources checked on **2026-10-16**; requirements refer to the upstream versions reviewed on that date.

## Block-sparse Model Kernels

### MSA: MiniMax Sparse Attention

- [Paper](https://arxiv.org/abs/2606.13392) | [Official code](https://github.com/MiniMax-AI/MSA) | [CuTe-DSL implementation guide](https://github.com/MiniMax-AI/MSA/blob/main/python/fmha_sm100/cute/README.md).
- **Sparsity:** A lightweight index branch chooses top-k KV blocks independently for each GQA group; the main branch attends to those blocks. The paper describes exponentiation-free selection and KV-outer execution.
- **Kernel coverage:** The `fmha_sm100` package contains CUDA C++ proxy/selection kernels and CuTe-DSL sparse forward kernels, including sparse prefill and paged FP8 decode. The public surface includes `sparse_topk_select`, `sparse_atten_func`, and `SparseDecodePagedAttentionWrapper`.
- **Boundary:** The current official repository requires NVIDIA SM100 and Linux. The paper's H800 experiments do not establish Hopper compatibility for this release. The sparse CuTe-DSL path is forward-only; do not infer a public sparse backward kernel from the paper's model-training recipe.
- **License:** MIT for MiniMax-authored code; bundled and derived components retain their own licenses.

### MoBA and FlashMoBA

- [MoBA paper](https://arxiv.org/abs/2502.13189) | [Moonshot reference implementation](https://github.com/MoonshotAI/MoBA).
- **Sparsity:** Queries route to relevant KV blocks using parameter-free top-k gating. Moonshot explicitly requires continued training when adapting an existing dense model.
- [FlashMoBA paper: Optimizing Mixture of Block Attention](https://arxiv.org/abs/2511.11571) | [Official FlashMoBA kernels](https://github.com/mit-han-lab/flash-moba).
- **Kernel coverage:** CUDA attention kernels designed to make small MoBA blocks practical. `flash_moba_varlen_func` combines routing and attention for packed variable-length sequences; `flash_topk_varlen_func` and `flash_moba_attn_varlen_func` expose the two stages separately.
- **Boundary:** The reviewed FlashMoBA release is based on FlashAttention 2.8.3 and requires CUDA 12.9+ and PyTorch 2.8+. Keep the reference model implementation distinct from the optimized kernel backend.

### NSA and FSA

- [NSA paper](https://arxiv.org/abs/2502.11089) | [FLA Triton implementation](https://github.com/fla-org/native-sparse-attention).
- **Sparsity:** NSA combines compressed context, selected fine-grained KV blocks, and a sliding window. Compression, selection, and branch weighting belong to the trained model, not just the attention multiply.
- **Kernel coverage:** FLA's independent implementation provides online top-k selection, fused selected/sliding attention, and training integration. It is not labelled as DeepSeek's official kernel release.
- [FSA paper](https://arxiv.org/abs/2508.18224) | [Official FSA kernels](https://github.com/Relaxed-System-Lab/Flash-Sparse-Attention).
- **Kernel design:** FSA places KV blocks in the outer loop and groups queries that share a block. Separate partial-attention, reduction, and softmax-statistics kernels reduce padding work when GQA groups are small.
- **Boundary:** FSA covers training and prefill; its repository labels one-step decoding in `fsa_preview` as beta. NSA, FSA, and DeepSeek Sparse Attention (DSA) are not interchangeable names.

## Token-sparse Inference Kernels

### FlashMLA and DeepSeek Sparse Attention

- [Official code and requirements](https://github.com/deepseek-ai/FlashMLA) | [DSA introduction](https://github.com/deepseek-ai/DeepSeek-V3.2-Exp).
- **Sparsity:** An indexer selects individual KV tokens; FlashMLA consumes their indices for sparse prefill and decoding. Token-level selection differs from MSA/MoBA's block routing.
- **Kernel coverage:** Sparse prefill, FP8/FP4-cache decode, and fused attention-adjacent operations. `flash_mla_with_kvcache` consumes the selected-token `indices`; the library also contains dense kernels, which must not be treated as sparse benchmarks.
- **Version boundary:** The 2026-09-30 release removed Hopper and older-model support and changed KV-cache layouts. The current NVIDIA backend requires SM100/SM103, CUDA 13.1+, and PyTorch 2.10+; an Ascend 950 backend is also documented. Use the historical revision linked in the upstream README for Hopper or older DeepSeek models.

## General Block-sparse Backends

### MIT HAN Lab Block-Sparse-Attention

- [Code](https://github.com/mit-han-lab/Block-Sparse-Attention).
- **Patterns:** Supplied block masks, token/block streaming attention, and different patterns per head; forward and backward kernels are available.
- **Boundary:** The upstream update documents Hopper and Blackwell support. This backend executes a supplied pattern; it does not supply MSA's learned index branch or MoBA's routing algorithm.

### Flex-Block-Attn

- [Code](https://github.com/Tencent-Hunyuan/flex-block-attn).
- **Patterns:** Block-mask-driven attention developed for Hunyuan Video, based on ThunderKittens and optimized for Hopper/SM90.
- **Boundary:** The documented kernel requires head dimension 128, aligned query/KV tile lengths, and block-level masks. Inspect the actual exported kernels before assuming every advertised video-attention strategy is released.

### PyTorch FlexAttention and FlashInfer

- [FlexAttention introduction](https://pytorch.org/blog/flexattention/) - `torch.compile` generates fused attention kernels; `create_block_mask` builds the `BlockMask` needed to skip masked blocks. A score modification alone does not guarantee sparse execution.
- [FlashInfer sparse APIs](https://docs.flashinfer.ai/api/sparse.html) - `BlockSparseAttentionWrapper` and `VariableBlockSparseAttentionWrapper` execute supplied sparse layouts. The documented MSA operators provide a separate model-specific path; their hardware coverage should not be inferred from MiniMax's `fmha_sm100` package.

## Paper Reading Order

Dates are the first arXiv submission months, not repository release dates.

- **2025-02:** [NSA: Hardware-Aligned and Natively Trainable Sparse Attention](https://arxiv.org/abs/2502.11089).
- **2025-02:** [MoBA: Mixture of Block Attention for Long-Context LLMs](https://arxiv.org/abs/2502.13189).
- **2025-08:** [FSA: An Alternative Efficient Implementation of Native Sparse Attention Kernel](https://arxiv.org/abs/2508.18224).
- **2025-11:** [Optimizing Mixture of Block Attention](https://arxiv.org/abs/2511.11571).
- **2026-06:** [MiniMax Sparse Attention](https://arxiv.org/abs/2606.13392).

## Evaluation Boundaries

Compare dense and sparse kernels under stated GPU, software revision, dtype, sequence lengths, head dimensions, GQA ratio, block size, selected-token/block budget, and KV-cache layout. Measure selection, mask construction, attention, and reduction costs separately and together; distinguish prefill, decode, forward, and backward. Kernel latency and model quality are separate measurements, and a sparse model is not automatically a drop-in replacement for a dense checkpoint.

[Back to Sparse Attention Kernels](../../README.md#sparse-attention-kernels) | [Back to AI Infra](../../README.md#ai-infra)
