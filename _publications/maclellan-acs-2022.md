---
layout: publication
scholar_meta: true
title: "Efficient Induction of Language Models via Probabilistic Concept Formation"
authors:
  - {first: "Christopher J.", last: "MacLellan"}
  - {first: "Peter", last: "Matsakis"}
  - {first: "Pat", last: "Langley"}
year: 2022
date: 2022-06-01
venue: "Proceedings of the Tenth Annual Conference on Advances in Cognitive Systems"
venue_short: "ACS 2022"
venue_type: conference
doi: ""
pdf_url: "https://chrismaclellan.com/media/publications/maclellan-acs-22.pdf"
arxiv_url: "https://arxiv.org/abs/2212.11937"
video_url: "https://www.youtube.com/watch?v=ACTJaLlup-I"
poster_url: ""
website_url: "https://advancesincognitivesystems.github.io/acs2022/data/acs22_paper-2537.pdf"
award: ""
topics:
  - Language Modeling
  - Concept Learning
projects:
  - cobweb
abstract: "This paper presents a novel approach to the acquisition of language models from corpora. The framework builds on Cobweb, an early system for constructing taxonomic hierarchies of probabilistic concepts that used a tabular, attribute-value encoding of training cases and concepts, making it unsuitable for sequential input like language. In response, we explore three new extensions to Cobweb—the Word, Leaf, and Path variants. These systems encode each training case as an anchor word and surrounding context words, and they store probabilistic descriptions of concepts as distributions over anchor and context information. As in the original Cobweb, a performance element sorts a new instance downward through the hierarchy and uses the final node to predict missing features. Learning is interleaved with performance, updating concept probabilities and hierarchy structure as classification occurs. Thus, the new approaches process training cases in an incremental, online manner that it very different from most methods for statistical language learning. We examine how well the three variants place synonyms together and keep homonyms apart, their ability to recall synonyms as a function of training set size, and their training efficiency. Finally, we discuss related work on incremental learning and directions for further research."
---
