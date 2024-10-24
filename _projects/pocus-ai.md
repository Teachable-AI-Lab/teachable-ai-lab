---
title: POCUS AI
layout: post
img: /images/pocus-pnb.gif
date: 2022-09-28
comments: false
order: 1
category: past
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

Hannan, D., Nesbit, S. C., Wen, X., Smith, G., Zhang, Q., Goffi, A., Chan, V., Morris,
M.J., Hunninghake, J. C., Villalobos, N. E., Kim, E., Weber, R. O., & MacLellan, C. J.
(2024). Interpretable Models for Detecting and Monitoring Elevated Intracranial Pressure.
In _Proceedings of The International Symposium on Biomedical Imaging_.
[<i class="far fa-file-pdf"></i>][hannan-isbi-24]

[hannan-isbi-24]: https://ieeexplore.ieee.org/document/10635474

Hannan, D., Nesbit, S.C., Wen, X., Smith, G., Zhang, Q., Goffi, A., Chan, V., Morris, M.J., 
Hunninghake, J.C., Villalobos, N.E., Kim, E., Weber, R.O., MacLellan, C.J. (2023). 
MobilePTX: Sparse Coding for Pneumothorax Detection Given Limited Training Examples. In Proceedings
of The Thirty-Fifth Annual Conference on Innovative Applications of Artificial Intelligence.
[<i class="far fa-file-pdf"></i>][hannan-iaai-23] [<i class="fab fa-youtube"></i>][hannan-iaai-23-video]

[hannan-iaai-23]: https://ojs.aaai.org/index.php/AAAI/article/view/26859
[hannan-iaai-23-video]: https://youtu.be/7ex8qQT5xSs

Smith, G.\*, Zhang, Q.\*, MacLellan, C.J. (2022). Do it Like the Doctor: How We
Can Design a Model That Uses Domain Knowledge to Diagnose Pneumothorax. In
Proceedings of the AAAI 2022 Spring Symposium on Machine Learning and Knowledge
Engineering for Hybrid Intelligence (AAAI-MAKE 2022). (* are co-first authors)
[<i class="far fa-file-pdf"></i>][smith-make-22] [<i class="fab fa-youtube"></i>][smith-make-22-talk]

[smith-make-22-talk]: https://www.youtube.com/watch?v=hKtjlMX9n0c
[smith-make-22]: https://doi.org/10.48550/arXiv.2205.12159
