# SF2DA: Adapting Segment Anything Model with Spatial-Frequency Dual-Domain Adapter for Medical Image Segmentation

## Overview

SAM provides general-purpose visual representations, but the domain gap between natural and medical images makes task-specific adaptation challenging. SF2DA introduces a **Spatial-Frequency Dual-Branch Adapter (SFDBA)** into the SAM image encoder while keeping the pretrained SAM image encoder, prompt encoder, and mask decoder frozen.

The adapter combines:

* **Local Spatial Branch (LSB):** Uses depthwise and pointwise convolutions to model local structural patterns.
* **Frequency-domain Global Branch (FGB):** Applies a two-dimensional real fast Fourier transform (RFFT), learnable channel-wise complex-valued full-spectrum filtering, and inverse RFFT to obtain frequency-enhanced features with a global receptive field.
* **Bidirectional Information Interaction Module (BIIM):** Performs cross-attention in both directions between local and frequency-enhanced features before fusion.
* **Spatial Gated Fusion (SGF):** Learns spatially varying gates to adaptively combine the two interacted features.

The SFDBA is inserted in parallel with the MLP pathway of each Transformer block in the SAM ViT-B image encoder. **Only SFDBA parameters are trained.**

<!-- TODO: Add an architecture illustration once its file has been added to the repository. -->

## Main results

Mean ± standard deviation across three independent runs, as reported in the manuscript. Metrics below are percentages.

| Dataset | Dice (%) | IoU (%) | Accuracy (%) | Precision (%) | Recall (%) |
| --- | --- | --- | --- | --- | --- |
| BUSI | 86.88 ± 0.07 | 78.74 ± 0.23 | 97.79 ± 0.06 | 88.45 ± 0.98 | 88.25 ± 0.76 |
| CVC-ClinicDB | 94.34 ± 0.22 | 89.69 ± 0.40 | 99.23 ± 0.05 | 94.64 ± 0.52 | 94.41 ± 0.07 |
| ISIC2018 | 90.55 ± 0.28 | 83.81 ± 0.35 | 96.16 ± 0.10 | 91.40 ± 0.44 | 91.98 ± 0.79 |

**Trainable parameters:** 5.28 million (as reported in the manuscript).

## Pretrained model

The experiments use the pretrained **SAM ViT-B** backbone initialized from SAM weights trained on SA-1B. The SAM image encoder, prompt encoder, and mask decoder remain frozen during SF2DA training.
