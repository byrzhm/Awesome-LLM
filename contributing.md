# Contribution Guidelines

This repository follows a **LLM foundations -> AI Infra** learning path. Keep resources in two categories: **LLM** for milestone papers, model architectures, pretraining and post-training algorithms, datasets, model quality evaluation, applications, and learning materials; **AI Infra** for distributed training, RL training systems, inference and serving, GPU kernels and compilers, communication, cluster orchestration, data pipelines, profiling, benchmarks, and observability.

To add, remove, or change things on this repository please submit a pull request that adheres to the following guidelines:

- Put resources in the most relevant subsection. Prefer cross-links to duplicate entries; overview sections such as **Milestone Papers**, **Recent Research**, and **AI Infra Highlights** may point to detailed topic guides.
- Store model and algorithm paper guides in `paper_list/llm/` and systems paper guides in `paper_list/ai_infra/`. Use lowercase, snake_case filenames and update relative links and README navigation when moving a file.
- Classify by the primary contribution: PPO/GRPO and reward design belong to **LLM**; rollout scheduling and distributed RL execution belong to **AI Infra**. Dataset collections and model quality belong to **LLM**; data-processing pipelines and systems performance belong to **AI Infra**. Cross-link resources spanning both categories.
- Prefer official papers, documentation, and maintained implementation repositories. Include a concise description of the model/algorithm contribution or infrastructure problem addressed.
- For **Milestone Papers**, explain the model or algorithm contribution. Put systems milestones in **AI Infra Papers**, with their bottlenecks and engineering contributions. Sort papers chronologically within each category, using the first submission date and explicitly labeling official-release reports.
- For performance claims, cite the source and state the hardware, workload, precision, software versions, and measurement conditions. Distinguish latency, throughput, memory use, model quality, and cost rather than treating them as interchangeable.
- For model checkpoints and datasets, link to the official source and include available licensing or access restrictions relevant to deployment and training.
- Please follow the existing format to add new terms.

Thank you for your suggestions!

## Updating your PR

A lot of times, making a PR adhere to the standards above can be difficult.
If the maintainers notice anything that we'd like changed, we'll ask you to
edit your PR before we merge it. There's no need to open a new PR, just edit
the existing one. If you're not sure how to do that,
[here is a guide](https://github.com/RichardLitt/knowledge/blob/master/github/amending-a-commit-guide.md)
on the different ways you can update your PR so that we can merge it.

## how to add dynamic citation badge

1. get paper id from semantic scholar paper page 
2. create dynamic badge at [this site](https://shields.io/badges/dynamic-json-badge) with this link: https://api.semanticscholar.org/graph/v1/paper/{paper_id}?fields=citationCount
3. ![Dynamic JSON Badge](https://img.shields.io/badge/dynamic/json?url=https%3A%2F%2Fapi.semanticscholar.org%2Fgraph%2Fv1%2Fpaper%2F432bef8e34014d726c674bc458008ac895297b51%3Ffields%3DcitationCount&query=%24.citationCount&label=citation)
