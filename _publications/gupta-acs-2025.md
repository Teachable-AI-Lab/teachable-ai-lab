---
layout: publication
scholar_meta: true
title: "Hierarchical Semantic Retrieval with Cobweb"
authors:
  - {first: "Anant", last: "Gupta"}
  - {first: "Karthik", last: "Singaravadivelan"}
  - {first: "Zekun", last: "Wang"}
year: 2025
date: 2025-06-01
venue: "Proceedings of the Twelfth Annual Conference on Advances in Cognitive Systems"
venue_short: "ACS 2025"
venue_type: conference
pages: "125-144"
doi: ""
pdf_url: "https://openreview.net/pdf?id=ms3utEpR78"
arxiv_url: ""
video_url: ""
poster_url: ""
website_url: "https://openreview.net/forum?id=ms3utEpR78"
award: ""
topics:
  - Information Retrieval
projects:
  - cobweb
abstract: "Neural document retrieval often treats a corpus as a flat cloud of vectors scored at a single granularity, leaving corpus structure underused and explanations opaque. We use Cobweb--a hierarchy-aware framework--to organize sentence embeddings into a prototype tree and rank documents via coarse-to-fine traversal. Internal nodes act as concept prototypes, providing multi-granular relevance signals and transparent rationale through retrieval paths. We instantiate two inference approaches: a generalized best-first search and a lightweight path-sum ranker. We evaluate our approaches on MS MARCO and QQP with encoder (e.g., BERT/T5) and decoder (GPT-2) representations. Our results show that our retrieval approaches match the dot product search on strong encoder embeddings while remaining robust when kNN degrades: with GPT-2 vectors, dot product performance collapses whereas our approaches still retrieve relevant results. Overall, our experiments suggest that Cobweb provides competitive effectiveness, improved robustness to embedding quality, scalability, and interpretable retrieval via hierarchical prototypes."
---
