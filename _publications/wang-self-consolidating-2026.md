---
layout: publication
scholar_meta: true
title: "Self-Consolidating Language Models: Continual Knowledge Incorporation from Context"
authors:
  - {first: "Zekun", last: "Wang"}
  - {first: "Anant", last: "Gupta"}
  - {first: "Zihan", last: "Dong"}
  - {first: "Christopher J.", last: "MacLellan"}
year: 2026
date: 2026-5-08
venue: "arXiv preprint"
venue_short: "arXiv"
venue_type: preprint
pages: ""
doi: ""
pdf_url: ""
arxiv_url: "https://arxiv.org/abs/2605.07076"
video_url: ""
poster_url: ""
website_url: ""
award: ""
topics:
  - Compositionality
  - Diffusion
projects:
  - cobweb
abstract: >-
  Large language models (LLMs) increasingly receive information as streams of passages, conversations, and long-context workflows. While longer context windows expose more evidence, they do not ensure that useful information is preserved and reused. We study continual context consolidation: writing current context into model weights while limiting interference with previously consolidated information. We propose \textbf{S}elf-\textbf{Co}nsolidating \textbf{L}anguage Models (SCoL), a post-training framework in which, given current context, an LLM learns to generate textual update instructions specifying which of its own Transformer layers should be updated. Because committed updates change the model that later generates future selections, we train SCoL with meta-reinforcement learning over an evolving model state. We instantiate SCoL with supervised QA rewards on SQuAD knowledge incorporation and intrinsic likelihood-based rewards for LongBench v2 long-context consolidation. Across both settings, SCoL improves acquisition and retention over prompting, summarization, batch test-time training, and sequential finetuning baselines. Analysis of learned selection patterns shows that SCoL encourages the LLM to generate sparse update locations that align with layers of high Fisher information, suggesting that the model learns to route plasticity toward loss-sensitive regions while limiting interference. Moreover, SCoL transfers from shorter meta-training streams to longer LongBench v2 streams at evaluation, suggesting that our framework supports scalable streaming consolidation.
---
