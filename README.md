# ATDA++: Shared Feature Representation with Tri-Network Adaptive Learning for Cross-Domain Recognition

Official public repository for the manuscript:

> **ATDA++: Shared Feature Representation with Tri-Network Adaptive Learning for Cross-Domain Recognition**  
> Musfequa Rahman, Md. Al-Mamun Provath, Kaushik Deb, Md. Khairul Alam Mazumder, Mohsin Ibna Hossain, Md Rakibul Islam\*, and M. F. Mridha  
> \*Corresponding author: `md.rakibul.islam@mdu.se`

---

## Pre-Publication Notice (Data and Code Availability)

This repository is currently in **pre-publication** status while the manuscript is under peer review at *Machine Learning with Applications* (Elsevier).

In accordance with the manuscript's **Data and Code Availability** statement:
- **Benchmark Datasets**: All benchmark datasets analyzed in this study (MNIST, USPS, SVHN, MNIST-M, and Office-31) are publicly available through their canonical academic repositories (detailed below).
- **Source Code and Evaluation Scripts**: Full source code and evaluation scripts for ATDA++ are available from the corresponding author (`md.rakibul.islam@mdu.se`) upon reasonable request during review, and will be openly released in this repository upon publication.

---

## Method Overview

**ATDA++** extends Asymmetric Tri-training Domain Adaptation (ATDA) with four modular components addressing four concurrent failure modes in pseudo-label tri-training:

1. **Uncertainty-Calibrated Tri-Head Agreement (UCTA)**: Replaces fixed-threshold confidence gating with a joint margin–entropy confidence weight.
2. **Dynamic Weight Constraint Decay (DWCD)**: Ties the labelling-head diversity penalty ($\lambda_{\mathrm{WC}}$) to measured pseudo-label accuracy via an exponential schedule.
3. **Class-Balanced Curriculum Pool Expansion (CB-CPE)**: Enforces per-class quotas during pool construction to prevent minority-class starvation.
4. **Source Prototype Anchor Regularisation (SPAR)**: Anchors the target classifier head ($F_t$) exclusively to source class prototypes via stop-gradient, leaving the shared backbone and labelling heads unaffected.

---

## Benchmark Datasets & Access

| Dataset | Domain / Task | Protocol in Manuscript | Canonical Source |
| :--- | :--- | :--- | :--- |
| **MNIST** | Handwritten digits | P1 (2,000 train), P2 (full split), SVHN→MNIST, MNIST→MNIST-M | LeCun et al. (1998) — `torchvision.datasets.MNIST` |
| **USPS** | Handwritten digits | P1 (1,800 train), P2 (full split) | Denker et al. (1989) — `torchvision.datasets.USPS` |
| **SVHN** | Street-view house numbers | SVHN→MNIST | Netzer et al. (2011) — `torchvision.datasets.SVHN` |
| **MNIST-M** | Blended color-patch digits | MNIST→MNIST-M (DANN protocol) | Ganin et al. (2016) |
| **Office-31** | 31 object classes (4,652 images across Amazon, Webcam, DSLR) | All 6 directed transfer tasks (ResNet-50) | Saenko et al. (2010) / Long et al. (2015) |

---

## Experimental Configurations & Reproducibility Guidelines

All experimental hyperparameters and training settings reported in the manuscript (Section *Implementation Details*):

- **Random Seed**: `42` (fixed across all experiments)
- **Evaluation Metric**: Classification accuracy on the target test set (no validation set used for early stopping)
- **Digit Architecture**:
  - Shared backbone $F$: 2 Conv–BN–Pool blocks (`768-d` output)
  - Labelling heads $F_1, F_2$ (`LabelNet`): dropout $p = 0.5$
  - Target head $F_t$ (`TargetNet`): dropout $p = 0.2$
- **Office-31 Architecture**: ResNet-50 backbone replacing $F$ (standard fine-tuning protocol)
- **Optimizer**: SGD with momentum `0.9`, learning rate `0.01`, weight decay `1e-4`
- **Fixed Digit Hyperparameters** (no task-specific tuning):
  - $\tau_0 = 0.90$
  - $\alpha = 0.5$
  - $\lambda_{\max} = 0.01$, $\lambda_{\min} = 10^{-4}$
  - $\gamma = 3.0$
  - $\lambda_{\mathrm{SPAR}} = 0.05$
  - $T = 30$ iterations

---

## Summary of Reported Results

| Benchmark / Protocol | Reported Target Accuracy |
| :--- | :---: |
| MNIST $\to$ USPS (Protocol 1) | `0.9576` |
| USPS $\to$ MNIST (Protocol 1) | `0.9789` |
| SVHN $\to$ MNIST | `0.9112` |
| MNIST $\to$ MNIST-M | `0.9929` |
| Office-31 (ResNet-50, 6-task mean) | `88.9%` |

---

## Citation

If you refer to this work prior to publication, please cite:

```bibtex
@article{rahman2026atdapp,
  title   = {ATDA++: Shared Feature Representation with Tri-Network Adaptive Learning for Cross-Domain Recognition},
  author  = {Rahman, Musfequa and Provath, Md. Al-Mamun and Deb, Kaushik and Mazumder, Md. Khairul Alam and Hossain, Mohsin Ibna and Islam, Md Rakibul and Mridha, M. F.},
  journal = {Machine Learning with Applications},
  year    = {2026},
  note    = {Under review}
}
```
