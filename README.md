# Teacher-Guided Recovery for GETA-Based Joint Structured Pruning and Quantization: A Comparative Study of Fixed and Compression-Aware Distillation

> **Paper submitted to Neurocomputing (Elsevier), 2026**  
> Author: MD. Sabbir Alam Pial, Ayesha Akter Lima, Abu Raihan, Al Reyad | Institution: Varendra University | Country: Bangladesh

---

## Overview

This repository contains all experimental results, training logs, configuration details, and figures for the paper:

**"Teacher-Guided Recovery for GETA-Based Joint Structured Pruning and Quantization: A Comparative Study of Fixed and Compression-Aware Distillation"**

We study teacher-guided recovery for GETA-based joint structured pruning and weight-only quantization by comparing three methods:

- **GETA-only** — cross-entropy loss only
- **GETA + Vanilla KD** — fixed knowledge distillation
- **GETA + CAKR** — compression-aware adaptive knowledge regularization 

---

## Key Results

All experiments use 60% target group sparsity, weight-only quantization (8–16 bit), 150 epochs, and 3 random seeds (123, 234, 345).

| Model / Dataset      | GETA-only    | Vanilla KD       | CAKR             | Best       |
| -------------------- | ------------ | ---------------- | ---------------- | ---------- |
| ResNet20 / CIFAR-10  | 87.90 ± 0.61 | 87.64 ± 0.34 ⚠️  | **88.05 ± 0.13** | CAKR       |
| ResNet56 / CIFAR-10  | 93.15 ± 0.19 | 93.22 ± 0.05     | **93.26 ± 0.15** | CAKR       |
| VGG7 / CIFAR-10      | 90.75 ± 0.22 | **91.09 ± 0.13** | 90.88 ± 0.20     | Vanilla KD |
| ResNet20 / CIFAR-100 | 50.91 ± 0.83 | 51.44 ± 0.67     | **51.74 ± 0.60** | CAKR       |
| ResNet56 / CIFAR-100 | 67.68 ± 0.26 | **68.59 ± 0.09** | 68.21 ± 0.17     | Vanilla KD |
| VGG7 / CIFAR-100     | 63.57 ± 0.24 | **64.08 ± 0.46** | 63.98 ± 0.21     | Vanilla KD |

⚠️ Vanilla KD falls **below** GETA-only baseline on ResNet20/CIFAR-10 (87.64% vs 87.90%), while CAKR achieves the best accuracy (88.05%). This demonstrates that fixed KD can over-regularize smaller compressed students.

**CAKR improves over GETA-only in all 6 settings without exception.**

---

## Dense Baseline Metrics

| Model / Dataset      | Dense Acc. (%) | Params  | MACs    |
| -------------------- | -------------- | ------- | ------- |
| ResNet20 / CIFAR-10  | 92.96          | 272.47K | 41.00M  |
| ResNet56 / CIFAR-10  | 94.29          | 855.77K | 125.75M |
| VGG7 / CIFAR-10      | 92.62          | 1.15M   | 606.00M |
| ResNet20 / CIFAR-100 | 69.14          | 278.32K | 40.82M  |
| ResNet56 / CIFAR-100 | 73.26          | 861.62K | 125.75M |
| VGG7 / CIFAR-100     | 71.65          | 1.17M   | 152.79M |

## Experimental Setup

| Setting                 | Value                                 |
| ----------------------- | ------------------------------------- |
| Framework               | PyTorch 1.13.1 + torchvision 0.14.1   |
| Hardware                | 2× NVIDIA Tesla T4 (Kaggle)           |
| Target group sparsity   | 60%                                   |
| Quantization            | Weight-only, 8–16 bit                 |
| Compression epochs      | 150                                   |
| Optimizer               | SGD, lr=0.05, momentum=0.9, wd=5e-4   |
| Batch size              | 128                                   |
| Seeds                   | 123, 234, 345                         |
| Warm-up epochs          | 10                                    |
| Ramp epochs             | 50                                    |
| Vanilla KD temperature  | T = 3.0                               |
| Vanilla KD weight       | λ = 0.06 (CIFAR-10), 0.08 (CIFAR-100) |
| CAKR controller weights | w₁=0.35, w₂=0.30, w₃=0.10             |
| CAKR max KD weight      | {0.06, 0.08} per setting              |

---

## Training Notebooks (Kaggle)

All original training notebooks with timestamps are publicly available on Kaggle.

| Setting            | Method     | Seed | Notebook                                                                                    | Final Acc |
| ------------------ | ---------- | ---- | ------------------------------------------------------------------------------------------- | --------- |
| ResNet20/CIFAR-100 | Baseline   | 123  | [link](https://www.kaggle.com/code/mdrokyhasan/resnet20-cifar100-baseline)                  | 69.14%    |
| ResNet20/CIFAR-100 | CAKR       | 123  | [link](https://www.kaggle.com/code/mdrokyhasan/resnet20-cifar100-cakr)                      | 52.36%    |
| ResNet20/CIFAR-100 | CAKR       | 234  | [link](https://www.kaggle.com/code/sabbiralam1/seed-234-resnet20-cifar100-cakr)             | 51.17%    |
| ResNet20/CIFAR-100 | CAKR       | 345  | [link](https://www.kaggle.com/code/sabbiralam1/seed-345-resnet20-cifar100-cakr/)            | 51.69%    |
| ResNet20/CIFAR-100 | Vanilla KD | 123  | [link](https://www.kaggle.com/code/sabbiralam1/seed-123-resnet20-cifar100-vanila-kd)        | 52.21%    |
| ResNet20/CIFAR-100 | Vanilla KD | 234  | [link](https://www.kaggle.com/code/sabbiralam1/seed-234-resnet20-cifar100-vanila-kd)        | 51.09%    |
| ResNet20/CIFAR-100 | Vanilla KD | 345  | [link](hhttps://www.kaggle.com/code/sabbiralam1/seed-345-resnet20-cifar100-vanila-kd)       | 51.01%    |
| ResNet20/CIFAR-100 | GETA-only  | 123  | [link](https://www.kaggle.com/code/mdrokyhasan/resnet20-cifar100-geta-only)                 | 51.83%    |
| ResNet20/CIFAR-100 | GETA-only  | 234  | [link](https://www.kaggle.com/code/sabbiralam1/seed-234-resnet20-cifar100-geta-only)        | 50.21%    |
| ResNet20/CIFAR-100 | GETA-only  | 345  | [link](https://www.kaggle.com/code/sabbiralam1/seed-345-resnet20-cifar100-geta-only)        | 50.69%    |
| ResNet20/CIFAR-100 | GETA-only  | 345  | [link](https://www.kaggle.com/code/sabbiralampial/res56-cifar100-geta-only)                 | 67.57%    |
| ---                | ---        | ---  | ---                                                                                         | ---       |
| ResNet56/CIFAR-100 | Baseline   | 123  | [link](https://www.kaggle.com/code/sabbiralampial/resnet56-cifar100-baseline)               | 71.26%    |
| ResNet56/CIFAR-100 | CAKR       | 123  | [link](hhttps://www.kaggle.com/code/sabbiralampial/seed-123-cakr-res56-ci100)               | 68.40%    |
| ResNet56/CIFAR-100 | CAKR       | 234  | [link](https://www.kaggle.com/code/sabbiralam1/seed-234-resnet56-cifar100-cakr)             | 68.07%    |
| ResNet56/CIFAR-100 | CAKR       | 345  | [link](https://www.kaggle.com/code/sabbiralam1/seed-345-resnet56-cifar100-cakr)             | 68.16%    |
| ResNet56/CIFAR-100 | Vanilla KD | 123  | [link](https://www.kaggle.com/code/sabbiralam1/seed-123-resnet56-cifar100-vanilla-kd)       | 68.54%    |
| ResNet56/CIFAR-100 | Vanilla KD | 234  | [link](https://www.kaggle.com/code/sabbiralam1/seed-234-resnet56-cifar100-vanilla-kd)       | 68.54%    |
| ResNet56/CIFAR-100 | Vanilla KD | 345  | [link](https://www.kaggle.com/code/sabbiralam1/seed-345-resnet56-cifar100-vanilla-kd)       | 68.7%     |
| ResNet56/CIFAR-100 | GETA-only  | 123  | [link](https://www.kaggle.com/code/sabbiralampial/res56-cifar100-geta-only)                 | 67.57%    |
| ResNet56/CIFAR-100 | GETA-only  | 234  | [link](https://www.kaggle.com/code/sabbiralam1/seed-234-resnet56-cifar100-geta-only)        | 67.49%    |
| ResNet56/CIFAR-100 | GETA-only  | 345  | [link](https://www.kaggle.com/code/sabbiralam1/seed-345-resnet56-cifar100-geta-only)        | 67.97%    |
|                    |            |      |                                                                                             |           |
| VGG7/CIFAR-100     | Baseline   | 123  | [link](https://www.kaggle.com/code/mdrokyhasan/vgg7-cifar100-baseline)                      | 71.65%    |
| VGG7/CIFAR-100     | CAKR       | 123  | [link](https://www.kaggle.com/code/sabbiralam1/seed-123-vgg7-cifar100-cakr)                 | 64.15%    |
| VGG7/CIFAR-100     | CAKR       | 234  | [link](https://www.kaggle.com/code/sabbiralam1/seed-234-vgg7-cifar100-cakr)                 | 63.64%    |
| VGG7/CIFAR-100     | CAKR       | 345  | [link](https://www.kaggle.com/code/sabbiralam1/seed-345-vgg7-cifar100-cakr)                 | 64.05%    |
| VGG7/CIFAR-100     | Vanilla KD | 123  | [link](https://www.kaggle.com/code/sabbiralam1/seed-123-vgg7-cifar100-vanila-kd)            | 64.53%    |
| VGG7/CIFAR-100     | Vanilla KD | 234  | [link](https://www.kaggle.com/code/sabbiralam1/seed-234-vgg7-cifar100-vanila-kd)            | 64.08%    |
| VGG7/CIFAR-100     | Vanilla KD | 345  | [link](https://www.kaggle.com/code/sabbiralam1/seed-345-vgg7-cifar100-vanila-kd)            | 63.62%    |
| VGG7/CIFAR-100     | GETA-only  | 123  | [link](https://www.kaggle.com/code/sabbiralam1/seed-123-vgg7-cifar100-geta-only)            | 63.65%    |
| VGG7/CIFAR-100     | GETA-only  | 234  | [link](https://www.kaggle.com/code/sabbiralam1/seed-234-vgg7-cifar100-geta-only)            | 63.76%    |
| VGG7/CIFAR-100     | GETA-only  | 345  | [link](https://www.kaggle.com/code/sabbiralam1/seed-345-vgg7-cifar100-geta-only)            | 63.3%     |
|                    |            |      |                                                                                             |
| ResNet20/CIFAR-10  | baseline   | 123  | [link](https://www.kaggle.com/code/sabbiralampial/resnet20-basline)                         | 92.96%    |
| ResNet20/CIFAR-10  | CAKR       | 123  | [link](https://www.kaggle.com/code/sabbiralampial/seed-123-cakr-resnet20-cifar10)           | 87.98%    |
| ResNet20/CIFAR-10  | CAKR       | 234  | [link](https://www.kaggle.com/code/sabbiralampial/seed-234-cakr-geta-res20-ci10)            | 87.97%    |
| ResNet20/CIFAR-10  | CAKR       | 345  | [link](https://www.kaggle.com/code/sabbiralampial/seed-345-cakr-geta-res20-ci10)            | 88.2%     |
| ResNet20/CIFAR-10  | Vanilla KD | 123  | [link](https://www.kaggle.com/code/sabbiralampial/seed-123-res20-ci10-vanilla-kd)           | 87.73%    |
| ResNet20/CIFAR-10  | Vanilla KD | 234  | [link](https://www.kaggle.com/code/sabbiralampial/seed-234-res20-ci10-vanilla-kd)           | 87.26%    |
| ResNet20/CIFAR-10  | Vanilla KD | 345  | [link](https://www.kaggle.com/code/sabbiralampial/seed-345-res20-ci10-vanilla-kd)           | 87.92%    |
| ResNet20/CIFAR-10  | GETA-only  | 123  | [link](https://www.kaggle.com/code/sabbiralampial/seed-123-resnet20-cifar10-geta-only)      | 87.45%    |
| ResNet20/CIFAR-10  | GETA-only  | 234  | [link](https://www.kaggle.com/code/sabbiralampial/seed-234-resnet20-cifar10-geta-only)      | 87.65%    |
| ResNet20/CIFAR-10  | GETA-only  | 345  | [link](https://www.kaggle.com/code/sabbiralampial/seed-345-resnet20-cifar10-geta-onlym)     | 88.59%    |
|                    |            |      |                                                                                             |
| ResNet56/CIFAR-10  | Baseline   | 123  | [link](https://www.kaggle.com/code/sabbiralampial/resnet56-cifar10-basline)                 | 94.29%    |
| ResNet56/CIFAR-10  | CAKR       | 123  | [link](https://www.kaggle.com/code/sabbiralampial/seed-123-cakr-rs56-ci10)                  | 93.25%    |
| ResNet56/CIFAR-10  | CAKR       | 234  | [link](https://www.kaggle.com/code/sabbiralampial/seed-234-resnet56-cifar10-cakr)           | 93.19%    |
| ResNet56/CIFAR-10  | CAKR       | 345  | [link](https://www.kaggle.com/code/sabbiralampial/seed-345-resnet56-cifar10-cakr)           | 93.16%    |
| ResNet56/CIFAR-10  | Vanilla KD | 123  | [link](https://www.kaggle.com/code/sabbiralampial/seed-123-resnet56-cifar10-vanilla-kd)     | 93.26%    |
| ResNet56/CIFAR-10  | Vanilla KD | 234  | [link](https://www.kaggle.com/code/sabbiralam1/seed-234-resnet56-cifar10-vanilla-kd/output) | 93.23%    |
| ResNet56/CIFAR-10  | Vanilla KD | 345  | [link](https://www.kaggle.com/code/sabbiralam1/seed-345-resnet56-cifar10-vanilla-kd)        | 93.16%    |
| ResNet56/CIFAR-10  | GETA-only  | 123  | [link](https://www.kaggle.com/code/sabbiralampial/res56-ci10-geta-only)                     | 93.33%    |
| ResNet56/CIFAR-10  | GETA-only  | 234  | [link](https://www.kaggle.com/code/sabbiralampial/seed-234-res56-ci10-geta-only)            | 93.16%    |
| ResNet56/CIFAR-10  | GETA-only  | 345  | [link](https://www.kaggle.com/code/sabbiralampial/seed-345-res56-ci10-geta-only)            | 92.96%    |
|                    |            |      |                                                                                             |
| VGG7/CIFAR-10      | Baseline   | 123  | [link](https://www.kaggle.com/code/raihanvu/baseline-vgg7-cifer10)                          | 92.62%    |
| VGG7/CIFAR-10      | CAKR       | 123  | [link](https://www.kaggle.com/code/raihanvu/vgg7-ci10-cakr)                                 | 60.66%    |
| VGG7/CIFAR-10      | CAKR       | 234  | [link](https://www.kaggle.com/code/raihanvu/seed-234-vgg7-ci10-cakr)                        | 91.05%    |
| VGG7/CIFAR-10      | CAKR       | 345  | [link](https://www.kaggle.com/code/raihanvu/seed-345-vgg7-ci10-cakr/)                       | 90.94%    |
| VGG7/CIFAR-10      | Vanilla KD | 123  | [link](https://www.kaggle.com/code/sabbiralam1/seed-123-vgg7-cifar10-vanila-kd/)            | 91.04%    |
| VGG7/CIFAR-10      | Vanilla KD | 234  | [link](https://www.kaggle.com/code/sabbiralam1/seed-234-vgg7-cifar10-vanila-kd)             | 91.23%    |
| VGG7/CIFAR-10      | Vanilla KD | 345  | [link](https://www.kaggle.com/code/sabbiralampial/seed-345-vgg7-cifar10-vanilla-kd)         | 90.9%     |
| VGG7/CIFAR-10      | GETA-only  | 123  | [link](https://www.kaggle.com/code/raihanvu/vgg7-ci10-geta-only)                            | 90.63%    |
| VGG7/CIFAR-10      | GETA-only  | 234  | [link](https://www.kaggle.com/code/raihanvu/seed-234-vgg7-ci10-geta-only)                   | 91.0%     |
| VGG7/CIFAR-10      | GETA-only  | 345  | [link](https://www.kaggle.com/code/raihanvu/seed-345-vgg7-ci10-geta-only)                   | 90.61%    |

## Figures

| Figure                                         | Description                                       |
| ---------------------------------------------- | ------------------------------------------------- |
| ![Fig 2](./figures/cifar100v2.png)     | Training dynamics — ResNet20/CIFAR-100 (Seed 123) |
| ![Fig 3](./figures/output.png) | Accuracy comparison — all 6 settings              |
              

---

## Citation

If you use this work, please cite:

```bibtex
@article{yourname2025cakr,
  title   = {Teacher-Guided Recovery for GETA-Based Joint
Structured Pruning and Quantization: A Comparative
Study of Fixed and Compression-Aware Distillation},
  author  = {MD. Sabbir Alam Pial, Ayesha Akter Lima, Abu Raihan, Al Reyad},
  journal = {Neurocomputing},
  year    = {2026},
  note    = {Under review}
}
```

---

## Contact

**[MD. Sabbir Alam Pial]** — [Sabbir.vu11@gmail.com]  
**[Abu Raihan]** — [raihan.str13@gmail.com]  
**[Al Reyad]** — [Sabbir.vu11@gmail.com]  
Varendra University, Rajshahi, Bangladesh

---

_All experiments conducted on Kaggle (2× NVIDIA Tesla T4).
Results verified across 3 random seeds per setting.  
Training notebooks publicly available with original timestamps._
