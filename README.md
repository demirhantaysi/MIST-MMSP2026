[README.md](https://github.com/user-attachments/files/32576199/README.md)
# MIST — Masked Isolated Sharded disTillation

**A Hybrid Machine Unlearning Approach for Multimodal Data**

Alaattin Burak Koçer¹ · Demirhan Taysı¹ · Ender Mete Ekşioğlu¹ · Behçet Uğur Töreyin²

¹ Department of Electronics and Communication Engineering, Istanbul Technical University
² Department of Artificial Intelligence Engineering, Istanbul Technical University

Accepted at the **IEEE International Workshop on Multimedia Signal Processing (MMSP 2026)**, 22–24 September 2026, Istanbul, Türkiye.

---

## Overview

Data privacy regulations such as the GDPR establish a right to be forgotten, but deleting a record from a training set does not remove its influence from a model's weights. Machine unlearning addresses that gap. Most existing methods, however, are evaluated only on image classification, leaving their behaviour on other media unclear.

MIST is a class-level unlearning framework that combines two complementary ideas:

- **SISA-style sharded isolation** — the training set is split into disjoint shards, each training an independent sub-model, which localises the effect of any unlearning request.
- **DELETE-style masked knowledge distillation** — for a forget request, each affected shard model is frozen as a teacher, its forget-class logit is masked to a large negative value, and a student is trained to match the masked distribution with KL divergence plus a cross-entropy term on the remaining data.

A third stage, **entropy-weighted aggregation**, combines shard predictions at inference so that more confident (lower-entropy) shards carry more weight, which limits the influence of shards that were damaged by the unlearning step.

To the best of our knowledge, MIST is the first machine unlearning approach evaluated across image, audio and video modalities within a single framework.

## Experiments

**Method-level comparison** — MIST against nine unlearning baselines (Retrain from Scratch, SISA, DELETE, Fine-Tuning, Gradient Ascent, SCRUB, AmnesiacML, Boundary Shrink, Boundary Expanding) on CIFAR-10, MNIST, CIFAR-100 and FashionMNIST, with ResNet-18 as the shared backbone.

**Class-sweep generalisation** — every class unlearned in turn across six datasets and three modalities:

| Dataset | Modality | Classes swept | Backbone |
|---|---|---|---|
| CIFAR-10 | Image | 10 | ResNet-50 |
| MNIST | Image | 10 | ResNet-50 |
| CIFAR-100 | Image | 100 | ResNet-50 |
| FashionMNIST | Image | 10 | ResNet-50 |
| Speech Commands | Audio | 10 | Audio CNN |
| UCF101 | Video | 101 | 3D ResNet-18 |

**Metrics** — forget and retain accuracy on both splits, retain-side F1, H-Mean, Membership Inference Attack (MIA) success rate, unlearning time, and RAP (Remain Accuracy Preservation), a retain-accuracy preservation ratio defined in this work.

### Selected results (test split, forget class = 4)

| Dataset | Method | Acc_f ↓ | Acc_r ↑ | H-Mean ↑ | MIA ↓ | Time (s) ↓ |
|---|---|---|---|---|---|---|
| CIFAR-10 | Retrain from Scratch | 0.00% | 94.62% | 97.24 | 0.00% | 3925.2 |
| CIFAR-10 | SISA | 0.00% | 93.24% | 96.50 | 0.00% | 9405.0 |
| CIFAR-10 | DELETE | 0.00% | 94.98% | 97.42 | 0.00% | 626.6 |
| CIFAR-10 | **MIST (ours)** | **0.00%** | **95.18%** | **97.53** | **0.00%** | **160.1** |
| FashionMNIST | **MIST (ours)** | **0.00%** | **94.89%** | **97.38** | **0.00%** | **156.4** |

MIST reaches 0.00% test forget accuracy on all four image benchmarks while keeping retain-side accuracy at the level of the retrain-from-scratch gold standard, at a fraction of its cost. Full tables are in the paper.

## Repository contents

| File | Description |
|---|---|
| `2026270406.pdf` | Accepted version of the MMSP 2026 paper |

Code release is planned. Until then, please contact the authors with any questions about the implementation.

Pages and DOI will be added once the proceedings appear on IEEE Xplore.

## Acknowledgment

This work was supported by the Scientific and Technological Research Council of Türkiye (TÜBİTAK) under the 1515 Frontier R&D Laboratories Support Program for BTS Advanced AI Hub: BTS Autonomous Networks and Data Innovation Lab, Project 5239903; and partly by the Scientific Research Projects Coordination Department (BAP), Istanbul Technical University, under Project ITU-BAP MGA-2024-45372.

## Contact

{kocera21, taysi21, eksioglue, toreyin}@itu.edu.tr

---

## Copyright notice

© 2026 IEEE. Personal use of this material is permitted. Permission from IEEE must be obtained for all other uses, in any current or future media, including reprinting/republishing this material for advertising or promotional purposes, creating new collective works, for resale or redistribution to servers or lists, or reuse of any copyrighted component of this work in other works.

The PDF in this repository is the **accepted version** of the paper, not the final version published by IEEE Xplore. IEEE policy permits authors to post the accepted version on their own or their institution's servers, provided this notice is displayed. Once the paper appears on IEEE Xplore, this page will be updated with the full citation and a link to the published article.
