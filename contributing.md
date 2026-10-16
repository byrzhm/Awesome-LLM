# Contribution Guidelines

This repository is **AI Infra-first**. Prioritize resources about distributed training, reinforcement learning and post-training systems, inference and serving, GPU kernels and compilers, communication, cluster orchestration, data pipelines, profiling, benchmarks, and observability. LLMs and MoE models are the primary workloads; model and application resources should provide relevant context for infrastructure work.

To add, remove, or change things on this repository please submit a pull request that adheres to the following guidelines:

- Put resources in the most relevant section and avoid duplicate entries outside the **AI Infra Highlights** overview.
- Prefer official papers, documentation, and maintained implementation repositories. Include a concise description of the infrastructure problem addressed.
- For papers in **AI Infra Papers** or **Milestone Papers**, explain the systems contribution or relevance to AI infrastructure. Sort papers chronologically within each category, using the first submission date and explicitly labeling official-release reports.
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
