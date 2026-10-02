---
layout: publication
scholar_meta: true
title: "Trust Region Continual Learning is also a Meta-Learner"
authors:
  - {first: "Zekun", last: "Wang"}
  - {first: "Anant", last: "Gupta"}
  - {first: "Christopher J.", last: "MacLellan"}
year: 2026
date: 2026-12-08
venue: "Proceedings of the Fortieth Annual Conference on Neural Information Processing Systems"
venue_short: "NeurIPS 2026"
venue_type: conference
pages: ""
doi: ""
pdf_url: ""
arxiv_url: "https://arxiv.org/abs/2602.02417"
video_url: ""
poster_url: ""
website_url: ""
award: ""
topics:
  - Continual Learning
projects:
abstract: "Continual learning aims to acquire tasks sequentially without catastrophic forgetting, yet standard strategies face a core tradeoff: regularization-based methods (e.g., EWC) can overconstrain updates when task optima are weakly overlapping, while replay-based methods can retain performance but drift due to imperfect replay. We study a hybrid perspective: _trust region continual learning_ that combines generative replay with a Fisher-metric trust region constraint. We show that, under local approximations, the resulting update admits a MAML-style interpretation with a single implicit inner step: replay supplies an old-task gradient signal (query-like), while the Fisher-weighted penalty provides an efficient offline curvature shaping (support-like). This yields an emergent meta-learning property in continual learning: the model becomes an initialization that rapidly _re-converges_ to prior task optima after each task transition, without explicitly optimizing a bilevel objective. Empirically, on task-incremental diffusion image generation and continual diffusion-policy control, trust region continual learning achieves the best final performance and retention, and consistently recovers early-task performance faster than EWC, replay, and continual meta-learning baselines."
---
