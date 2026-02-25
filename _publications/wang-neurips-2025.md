---
layout: publication
scholar_meta: true
title: "Deep Taxonomic Networks for Unsupervised Hierarchical Prototype Discovery"
authors:
  - {first: "Zekun", last: "Wang"}
  - {first: "Ethan", last: "Haarer"}
  - {first: "Tianyi", last: "Zhu"}
  - {first: "Zhiyi", last: "Dai"}
  - {first: "Christopher J.", last: "MacLellan"}
year: 2025
date: 2025-12-01
venue: "Proceedings of Advances in Neural Information Processing Systems"
venue_short: "NeurIPS 2025"
venue_type: conference
doi: ""
pdf_url: "https://openreview.net/pdf?id=My72tmxg6t"
arxiv_url: "https://arxiv.org/abs/2509.23602"
video_url: ""
poster_url: ""
website_url: "https://neurips.cc/virtual/2025/loc/san-diego/poster/118417"
award: ""
topics:
  - Computer Vision
  - Concept Learning
projects:
  - cobweb
abstract: "Inspired by the human ability to learn and organize knowledge into hierarchical taxonomies with prototypes, this paper addresses key limitations in current deep hierarchical clustering methods. Existing methods often tie the structure to the number of classes and underutilize the rich prototype information available at intermediate hierarchical levels. We introduce deep taxonomic networks, a novel deep latent variable approach designed to bridge these gaps. Our method optimizes a large latent taxonomic hierarchy, specifically a complete binary tree structured mixture-of-Gaussian prior within a variational inference framework, to automatically discover taxonomic structures and associated prototype clusters directly from unlabeled data without assuming true label sizes. We analytically show that optimizing the ELBO of our method encourages the discovery of hierarchical relationships among prototypes. Empirically, our learned models demonstrate strong hierarchical clustering performance, outperforming baselines across diverse image classification datasets using our novel evaluation mechanism that leverages prototype clusters discovered at all hierarchical levels. Qualitative results further reveal that deep taxonomic networks discover rich and interpretable hierarchical taxonomies, capturing both coarse-grained semantic categories and fine-grained visual distinctions."
---
