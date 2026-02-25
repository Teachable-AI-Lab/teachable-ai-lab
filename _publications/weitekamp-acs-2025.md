---
layout: publication
scholar_meta: true
title: "CORGI: Efficient Pattern Matching With Quadratic Guarantees"
authors:
  - {first: "Daniel", last: "Weitekamp"}
year: 2025
date: 2025-06-01
venue: "Proceedings of the Twelfth Annual Conference on Advances in Cognitive Systems"
venue_short: "ACS 2025"
venue_type: conference
pages: "212-225"
doi: ""
pdf_url: "https://openreview.net/pdf?id=MQr2n8bxqU"
arxiv_url: ""
video_url: ""
poster_url: ""
website_url: "https://openreview.net/forum?id=MQr2n8bxqU"
award: ""
topics:
  - Pattern Matching
abstract: "Rule-based systems must solve complex matching problems within tight time constraints to be effective in real-time applications, such as planning and reactive control for AI agents, as well as low-latency relational database querying. Pattern-matching systems can encounter issues where exponential time and space are required to find all matches for rules with many underconstrained variables, or which produce combinatorial intermediate partial matches (but are otherwise well-constrained). Coding tricks for writing production rules and database queries can remediate many of these issues in practice when rules are handwritten. However, when online AI systems automatically generate rules from example-driven induction or code synthesis, they can easily produce rules with worst-case matching patterns that slow or halt program execution by exceeding available memory. In our own work with cognitive systems that learn from example, we've found that aggressive forms of anti-unification-based generalization can easily produce these circumstances. To make these systems practical without hand-engineering constraints or succumbing to unpredictable failure modes, we introduce a new matching algorithm called CORGI (Collection-Oriented Relational Graph Iteration). Unlike RETE-based approaches, CORGI offers quadratic time and space guarantees for finding single satisficing matches, and the ability to iteratively stream subsequent matches without committing entire conflict sets to memory. CORGI differs from RETE in that it does not have a traditional B-memory for collecting partial matches. Instead, CORGI takes a two-step approach: a graph of grounded relations is built/maintained in a forward pass, and an iterator generates matches as needed by working backward through the graph. This approach eliminates the high-latency delays and memory overflows that can result from populating full conflict sets. In a performance evaluation, we demonstrate that CORGI significantly outperforms RETE implementations from SOAR and OPS5 on a simple combinatorial matching task."
---
