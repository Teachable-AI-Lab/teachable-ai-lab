---
title: POCUS AI
layout: post
img: /images/gomoku_tutor.png
date: 2022-10-28
comments: false
---

The POCUS AI program will produce code demonstrating the feasibility of
automated interpretation of point- of-care ultrasound (POCUS) across multiple
applications and will provide a foundation for future work. POCUS can quickly
and accurately address a broad range of medical needs directly on the
battlefield when in the hands of highly-trained medical personnel. However, a
lack of POCUS competency among military medics has led to underutilization.
Automated interpretation by artificial intelligence (AI) could significantly
reduce training burdens and increase POCUS usage. The POCUS AI program will
address this need in a cost-effective manner by advancing AI techniques that
use minimal training data and can serve multiple distinct POCUS applications.
The algorithms will be demonstrated on four DoD-priority battlefield medical
needs: detection of pneumothorax, measurement of optic nerve sheath diameter,
nerve block guidance, and verification of endotracheal intubation. 

Our team is leveraging task-general POCUS domain knowledge obtained from
subject matter experts (SMEs) and unlabeled ultrasound images to generate
task-specific POCUS AI models from limited labeled training data. The approach
utilizes teachable AI, sparse coding, small-data classifiers, and
knowledge-based explainable AI to output timely results on mobile devices. The
final models can also explain their outputs, reducing medical diagnosis errors
(false negatives/positives) and increasing model adoption among military
medics.  

### Relevant Publications

Smith, G.*, Zhang, Q.*, MacLellan, C.J. (2022). Do it Like the Doctor: How We
Can Design a Model That Uses Domain Knowledge to Diagnose Pneumothorax. In
Proceedings of the AAAI 2022 Spring Symposium on Machine Learning and Knowledge
Engineering for Hybrid Intelligence (AAAI-MAKE 2022). (* are co-first authors)
[(pdf)][smith-make-22] [<i class="fab fa-youtube"></i>][smith-make-22-talk]

[smith-make-22-talk]: https://www.youtube.com/watch?v=hKtjlMX9n0c
[smith-make-22]: https://doi.org/10.48550/arXiv.2205.12159
