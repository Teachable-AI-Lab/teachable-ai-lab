---
layout: publication
scholar_meta: true
title: "Deliberate Planning in Language Models with Symbolic Representation"
authors:
  - {first: "Siheng", last: "Xiong"}
  - {first: "Zhangding", last: "Liu"}
  - {first: "Jieyu", last: "Zhou"}
  - {first: "Yuzhang", last: "Su"}
year: 2025
date: 2025-06-01
venue: "Poster Collection of the Twelfth Annual Conference on Advances in Cognitive Systems"
venue_short: "ACS 2025"
venue_type: conference
pages: "155-176"
doi: ""
pdf_url: "https://openreview.net/pdf?id=uJHpaZlIvT"
arxiv_url: ""
video_url: ""
poster_url: "https://openreview.net/forum?id=uJHpaZlIvT"
website_url: ""
award: ""
topics:
  - Large Language Models
abstract: "Planning remains a core challenge for large language models (LLMs), particularly in domains that require coherent multi-step action sequences grounded in external constraints. We introduce SymPlanner, a novel framework that equips LLMs with structured planning capabilities by interfacing them with a symbolic environment that serves as an explicit world model. Rather than relying purely on natural language reasoning, SymPlanner grounds the planning process in a symbolic state space, where a policy model proposes actions and a symbolic environment deterministically executes and verifies their effects. To enhance exploration and improve robustness, we introduce Iterative Correction (IC), which refines previously proposed actions by leveraging feedback from the symbolic environment to eliminate invalid decisions and guide the model toward valid alternatives. Additionally, Contrastive Ranking (CR) enables fine-grained comparison of candidate plans by evaluating them jointly. Conceptually, SymPlanner operationalizes two cognitive faculties: (i) error monitoring and repair via externalized feedback (IC) and (ii) preference formation among alternatives via pairwise comparison (CR), advancing cognitively plausible, symbol-grounded planning aligned with the rich structure in intelligent systems. We evaluate SymPlanner on PlanBench, demonstrating that it produces more coherent, diverse, and verifiable plans than pure natural language baselines."
---
