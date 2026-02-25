---
layout: publication
scholar_meta: true
title: "MobilePTX: Sparse Coding for Pneumothorax Detection Given Limited Training Examples"
authors:
  - {first: "Darryl", last: "Hannan"}
  - {first: "Steven C.", last: "Nesbit"}
  - {first: "Ximing", last: "Wen"}
  - {first: "Glen", last: "Smith"}
  - {first: "Qiao", last: "Zhang"}
  - {first: "Alberto", last: "Goffi"}
  - {first: "Vincent", last: "Chan"}
  - {first: "Michael J.", last: "Morris"}
  - {first: "John C.", last: "Hunninghake"}
  - {first: "Nicholas E.", last: "Villalobos"}
  - {first: "Edward", last: "Kim"}
  - {first: "Rosina O.", last: "Weber"}
  - {first: "Christopher J.", last: "MacLellan"}
year: 2023
date: 2023-02-01
venue: "Proceedings of the AAAI Conference on Artificial Intelligence"
volume: "37"
number: "13"
pages: "15675-15681"
venue_short: "IAAI 2023"
venue_type: conference
doi: "10.1609/aaai.v37i13.26859"
pdf_url: ""
arxiv_url: "https://arxiv.org/abs/2212.03282"
video_url: "https://youtu.be/7ex8qQT5xSs"
poster_url: ""
website_url: ""
award: ""
topics:
  - Computer Vision
  - Sparse Coding
projects:
  - pocus-ai
abstract: "Point-of-Care Ultrasound (POCUS) refers to clinician-performed and interpreted ultrasonography at the patient's bedside. Interpreting these images requires a high level of expertise, which may not be available during emergencies. In this paper, we support POCUS by developing classifiers that can aid medical professionals by diagnosing whether or not a patient has pneumothorax. We decomposed the task into multiple steps, using YOLOv4 to extract relevant regions of the video and a 3D sparse coding model to represent video features. Given the difficulty in acquiring positive training videos, we trained a small-data classifier with a maximum of 15 positive and 32 negative examples. To counteract this limitation, we leveraged subject matter expert (SME) knowledge to limit the hypothesis space, thus reducing the cost of data collection. We present results using two lung ultrasound datasets and demonstrate that our model is capable of achieving performance on par with SMEs in pneumothorax identification. We then developed an iOS application that runs our full system in less than 4 seconds on an iPad Pro, and less than 8 seconds on an iPhone 13 Pro, labeling key regions in the lung sonogram to provide interpretable diagnoses."
---
