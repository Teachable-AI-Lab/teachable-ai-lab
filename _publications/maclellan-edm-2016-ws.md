---
layout: publication
scholar_meta: true
title: "Investigating the Impact of Slipping Parameters on Additive Factors Model Parameter Estimates"
authors:
  - {first: "Christopher J.", last: "MacLellan"}
year: 2016
date: 2016-06-01
venue: "Proceedings of the Workshop and Tutorial Proceedings of EDM 2016"
venue_short: "EDM 2016"
venue_type: workshop
doi: ""
pdf_url: "https://chrismaclellan.com/media/publications/AFM_S_Workflow_Proposal_revised.pdf"
arxiv_url: ""
video_url: ""
poster_url: ""
website_url: "https://ceur-ws.org/Vol-1633/ws3-paper1.pdf"
award: ""
topics:
  - Knowledge Tracing
abstract: "The Additive Factors Model (AFM), a widely used model of student learning, estimates students’ prior knowledge, the difficulty of tutored skills, and the rates at which these skills are learned. In contrast to Bayesian Knowledge Tracing (BKT), another widely used model of student learning, AFM does not have parameters for the slipping rates of learned skills; i.e., it does not explicitly model situations where students know a skill, but still apply it incorrectly. Thus, AFM assumes that as students get more practice their probability of correctly applying a skill converges to 100%, whereas BKT allows convergence to lower probabilities. This restriction constrains the range of values that AFM parameters can take. In particular, when the asymptotic performance of a skill is less than 100%, AFM will estimate the learning rate to be lower than if slipping was taken into account.  To investigate this phenomenon, I will created a LearnSphere workflow component that implements AFM and a variant of AFM with explicit slipping parameters (AFM+S). Using this component, I analyze multiple DataShop datasets to determine (1) whether the model with slipping parameters better fits the data and (2) how the addition of slipping parameters impacts the parameter estimates returned by AFM. I show that, in general, AFM+S better fits the data than the AFM. Additionally, I show that AFM+S estimates higher skill intercepts and learning rates than AFM, whereas AFM estimates higher student intercepts than AFM+S."
---
