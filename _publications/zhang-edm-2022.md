---
layout: publication
scholar_meta: true
title: "Modifying Deep Knowledge Tracing for Multi-step Problems"
authors:
  - {first: "Qiao", last: "Zhang"}
  - {first: "Zeyu", last: "Chen"}
  - {first: "Natasha", last: "Lalwani"}
  - {first: "Christopher J.", last: "MacLellan"}
year: 2022
date: 2022-07-01
venue: "Proceedings of the 15th International Conference on Educational Data Mining"
venue_short: "EDM 2022"
venue_type: conference
pages: "684-688"
doi: "10.5281/zenodo.6853145"
pdf_url: "https://educationaldatamining.org/edm2022/proceedings/2022.EDM-posters.82/2022.EDM-posters.82.pdf"
arxiv_url: ""
video_url: "https://www.youtube.com/watch?v=pSmxlBQC76g"
poster_url: "https://chrismaclellan.com/media/publications/zhang-edm-22-poster.pdf"
website_url: ""
award: ""
topics:
  - Knowledge Tracing
projects:
  - investigating_kt_with_simstudents
abstract: "Previous studies suggest that Deep Knowledge Tracing (or DKT) has fundamental limitations that prevent it from supporting mastery learning on multi-step problems [15, 17].  Although DKT is quite accurate at predicting observed correctness in offline knowledge tracing settings, it often generates inconsistent predictions for knowledge components when used online. We believe this issue arises because DKT’s loss function does not evaluate predictions for skills and steps that do not have an observed ground truth value. To address this problem and enable DKT to better support online knowledge tracing, we propose the use of a novel loss function for training DKT. In addition to evaluating predictions that have ground truth observations, our new loss function also evaluates predictions for skills that do not have observations by using the ground truth label from the next observation of correctness for that skill. This approach ensures the model makes more consistent predictions for steps without observations, which are exactly the predictions that are needed to support mastery learning. We evaluated a DKT model that was trained using this updated loss by visualizing its predictions for a sample student learning sequence. Our analysis shows that the modified loss function produced improvements in the consistency of DKT model’s predictions."
---
