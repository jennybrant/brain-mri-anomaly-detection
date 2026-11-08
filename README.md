# Unsupervised Brain Tumor Detection in MRI: VAE vs. GAN

Can a generative model that has only ever seen **healthy** brain scans find tumors it was never trained on? We train models on healthy T2 MRI slices, then flag the regions they can't reconstruct as anomalies. We compare a **variational autoencoder (VAE)**, **f-AnoGAN** (a Wasserstein GAN with an encoder), a plain autoencoder and a simple intensity baseline, all under one shared pipeline.

Team project for **Introduction to Deep Learning and Generative Models (STAT 453), UW–Madison, Fall 2026**, by Jenny Brant, Abhijeet Beedikar, Diya Kothari and Daksh Kacham (equal contribution).

**Code:** the full codebase (preprocessing, models, training, evaluation, tests) is in the team repository, **[AbhijeetBeedikar/vae-vs-gan-brain-tumor-detection](https://github.com/AbhijeetBeedikar/vae-vs-gan-brain-tumor-detection)**.

## Data

- **Training:** 462 healthy subjects from the [IXI dataset](https://brain-development.org/ixi-dataset/) (T2).
- **Testing:** 436 glioma patients from the [Medical Segmentation Decathlon](http://medicaldecathlon.com/) (Task 01, Brain Tumour).

No tumor labels are used for training. They're only used to pick a detection threshold on a small validation set and to score the results.

## Preliminary results (64×64, single seed, 436 test patients)

| Model | Subject Dice | Pixel AUROC | Pixel AUPRC | Healthy false-positive rate |
| --- | --- | --- | --- | --- |
| **VAE** | **0.51** | **0.89** | **0.58** | **1.2%** |
| f-AnoGAN | 0.47 | 0.86 | 0.51 | 1.3% |
| Intensity baseline | 0.39 | 0.85 | 0.35 | 4.5% |
| Plain autoencoder | 0.38 | 0.80 | 0.40 | 4.0% |

- The generative models (VAE and f-AnoGAN) clearly beat the plain autoencoder and the "bright on T2" baseline.
- The plain autoencoder reconstructs *too* well: it partly copies tumors, so the residual misses them.
- f-AnoGAN has the fewest complete misses (8.7% of patients with Dice below 0.2, vs. 11.7% for the VAE).
- Small tumors are the main failure case.

**Next steps:** move to 128×128 resolution, add multiple seeds with confidence intervals, and sweep the VAE's KL weight and latent size.

## Files

- `docs/project_proposal.pdf` — project proposal.
- `docs/midway_report.pdf` — midway report (ICML 2026 template).
- `REPRODUCING.md` — a step-by-step guide to reproducing the project. The code it refers to is in the team repository linked above.
