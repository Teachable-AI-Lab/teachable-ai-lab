---
layout: publication
scholar_meta: true
title: "Efficient and Scalable Masked Word Prediction Using Concept Formation"
authors:
  - {first: "Xin", last: "Lian"}
  - {first: "Zekun", last: "Wang"}
  - {first: "Christopher J.", last: "MacLellan"}
year: 2025
date: 2025-01-01
venue: "Cognitive Systems Research"
venue_short: "Cognitive Systems Research 2025"
venue_type: journal
issn: "1389-0417"
doi: "10.1016/j.cogsys.2025.101371"
volume: "92"
pages: "101371"
pdf_url: ""
arxiv_url: ""
video_url: ""
poster_url: ""
website_url: ""
award: ""
topics:
  - Language Modeling
  - Concept Learning
projects:
  - cobweb
abstract: "This paper introduces Cobweb/4L, a novel approach for efficient language model learning that supports masked word prediction. The approach builds on Cobweb, an incremental system that learns a hierarchy of probabilistic concepts. Each concept stores the frequencies of words that appear in instances tagged with the concept label. The system utilizes an attribute-value representation to encode words and their context into instances. Cobweb/4L uses an information-theoretic variant of category utility as well as a new performance mechanism that leverages multiple concepts to generate predictions. We demonstrate that its new performance mechanism substantially outperforms prior Cobweb performance mechanisms that use only a single node to generate predictions. Further, we demonstrate that Cobweb/4L outperforms transformer-based language models in a low-data setting by learning more rapidly and achieving better final performance. Lastly, we show that Cobweb/4L, which is hyperparameter-free, is robust across varying scales of training data and does not require any manual tuning. This is in contrast to Word2Vec, which performs best with a varying number of hidden nodes that depend on the total amount of training data; this means its hyperparameters must be manually tuned for different amounts of training data. We conclude by discussing future directions for Cobweb/4L."
---
