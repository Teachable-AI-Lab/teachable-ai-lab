---
layout: publication
scholar_meta: true
title: "Test-Time Compositional Generalization in Diffusion Models via Concept Discovery"
authors:
  - {first: "Zekun", last: "Wang"}
  - {first: "Anant", last: "Gupta"}
  - {first: "Tianyi", last: "Zhu"}
  - {first: "Christopher J.", last: "MacLellan"}
year: 2026
date: 2026-5-08
venue: "arXiv preprint"
venue_short: "arXiv"
venue_type: preprint
pages: ""
doi: ""
pdf_url: ""
arxiv_url: "https://arxiv.org/abs/2605.07078"
video_url: ""
poster_url: ""
website_url: ""
award: ""
topics:
  - Compositionality
  - Diffusion
projects:
  - cobweb
abstract: "Compositional generalization requires models to produce novel configurations from familiar parts. In diffusion models, prior compositional generation methods typically assume that the relevant concepts or conditioning signals are already available. We instead ask whether a pretrained diffusion model can discover query-specific concepts from the time-indexed scores it learns for the noisy marginals $$p_t(x_t)$$ and compose them at test time. Given a single out-of-distribution query, our method performs gradient ascent on $$s_\theta(x_t,t) \approx \nabla_{x_t}\log p_t(x_t)$$ at multiple noising timesteps to recover local density modes, maps these modes into clean-space Gaussians, greedily selects relevant prototypes with a submodular likelihood objective, and combines them into a product-of-experts (PoE) teacher model with an analytic score. This teacher model can be sampled directly through classifier-free guidance or used to generate a sample pool for training a new class embedding and low-rank adapter. On held-out composition benchmarks built from ColorMNIST and CelebA, both the analytic PoE sampler and the low-rank adapted model outperform query-only and nearest trained-class baselines. These results suggest that the time-indexed score geometry of the diffusion model contains reusable density-mode concepts that support test-time compositional generation without a predefined concept library."
---
