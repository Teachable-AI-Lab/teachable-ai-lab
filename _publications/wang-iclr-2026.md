---
layout: publication
scholar_meta: true
title: "Avoid Catastrophic Forgetting with Rank-1 Fisher from Diffusion Models"
authors:
  - {first: "Zekun", last: "Wang"}
  - {first: "Anant", last: "Gupta"}
  - {first: "Zihan", last: "Dong"}
  - {first: "Christopher J.", last: "MacLellan"}
year: 2026
date: 2026-04-23
venue: "Proceedings of the Fourteenth International Conference on Learning Representations"
venue_short: "ICLR 2026"
venue_type: conference
doi: ""
pdf_url: ""
arxiv_url: "https://arxiv.org/abs/2509.23593"
video_url: ""
poster_url: ""
website_url: "https://openreview.net/forum?id=zCZcbRsc4g"
award: ""
topics:
  - Computer Vision
  - Concept Learning
  - Catastrophic Forgetting
  - Diffusion Models
abstract: "Catastrophic forgetting remains a central obstacle for continual learning in neural models. Popular approaches -- replay and elastic weight consolidation (EWC) -- have limitations: replay requires a strong generator and is prone to distributional drift, while EWC implicitly assumes a shared optimum across tasks and typically uses a diagonal Fisher approximation. In this work, we study the gradient geometry of diffusion models, which can already produce high-quality replay data. We provide theoretical and empirical evidence that, in the low signal-to-noise ratio (SNR) regime, per-sample gradients become strongly collinear, yielding an empirical Fisher that is effectively rank-1 and aligned with the mean gradient. Leveraging this structure, we propose a rank-1 variant of EWC that is as cheap as the diagonal approximation yet captures the dominant curvature direction. We pair this penalty with a replay-based approach to encourage parameter sharing across tasks while mitigating drift. On class-incremental image generation datasets (MNIST, FashionMNIST, CIFAR-10, ImageNet-1k), our method consistently improves average FID and reduces forgetting relative to replay-only and diagonal-EWC baselines. In particular, forgetting is nearly eliminated on MNIST and FashionMNIST and is more than halved on ImageNet-1k. These results suggest that diffusion models admit an approximately rank-1 Fisher. With a better Fisher estimate, EWC becomes a strong complement to replay: replay encourages parameter sharing across tasks, while EWC effectively constrains replay-induced drift."
---
