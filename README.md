# CLIP-DPM2W

Under review

## Overview

This repository contains:

- the implementation of **CLIP-DPM2W**
- the reconstructed **ML-FSAR benchmarks**
  - **SAV-MLFSAR**
  - **TinyVIRAT-v2-MLFSAR**
- dataset split files and preprocessing scripts for reproducible evaluation

**CLIP-DPM2W** is designed for **multi-label few-shot action recognition (ML-FSAR)**, where each video may contain multiple concurrent actions and only a few labeled examples are available for each class.

---

## News

- **[Dataset splits released]** Fixed train/test splits for reconstructed **SAV** and **TinyVIRAT-v2** are available in this repository.
- **[Code release]** Training and evaluation code for CLIP-DPM2W will be released here.

---

## Reconstructed ML-FSAR Benchmarks

Since there is no dedicated benchmark for multi-label few-shot action recognition, we reconstruct two existing multi-label action datasets into ML-FSAR benchmarks:

- **SAV-MLFSAR**
- **TinyVIRAT-v2-MLFSAR**

Both benchmarks use **fixed and disjoint base/novel class splits** and are evaluated under the same episodic protocol.

### 1. SAV-MLFSAR

**Original dataset:** SAV is a multi-person multi-label spatial action detection dataset composed of trimmed classroom video clips with frame-level person bounding boxes and action labels.

**How we reconstruct it:**
- Each annotated person instance is cropped **throughout the full clip duration**
- Each cropped person track is treated as an **independent sample**
- The resulting person-centric clip preserves the **full temporal duration** of the original trimmed video
- Action labels are directly inherited from the corresponding bounding-box annotations

**Statistics**
- **#clips:** 135,706
- **#classes:** 15
- **Base / Novel split:** 8 / 7

**Split rule**
- To avoid potential data leakage, clips derived from the **same source video** are kept in the **same split**

---

### 2. TinyVIRAT-v2-MLFSAR

**Original dataset:** TinyVIRAT-v2 is a multi-label action recognition dataset.

**How we reconstruct it:**
- We remove categories that do not contain enough samples for reliable **5-way 5-shot** episodic evaluation

**Removed categories**
- `activity_running`
- `vehicle_stopping`
- `Riding`
- `vehicle_starting`

**Statistics after filtering**
- **#videos:** 14,918
- **#classes:** 21
- **Base / Novel split:** 12 / 9

---

## Generalized ML-FSAR Setting

For both reconstructed benchmarks:

- base and novel classes are **mutually exclusive**
- the **meta-training split** contains only **base classes**
- the **evaluation split** is defined over the **joint label space** of base and novel classes

This means a test episode may contain:

- only base labels
- only novel labels
- or both base and novel labels

This generalized setting better reflects realistic multi-label videos, where seen and unseen actions may co-occur in the same clip.

---

## Evaluation Protocol

We follow the **generalized few-shot** evaluation setting and report results under:

- **5-way 1-shot**
- **5-way 3-shot**
- **5-way 5-shot**

### Metrics

Since this is a multi-label recognition task, we report:

- **mAP-base**: mean Average Precision over base classes
- **mAP-novel**: mean Average Precision over novel classes
- **HM**: harmonic mean between base and novel performance

\[
HM = \frac{2 \times mAP_{base} \times mAP_{novel}}{mAP_{base} + mAP_{novel}}
\]

---

## Dataset Downloads

### 1. Raw / processed benchmark data

Please download the reconstructed datasets from the links below:

#### SAV-MLFSAR
- **Processed benchmark:** `<SAV_MLFSAR_DOWNLOAD_LINK>`
- **Optional mirror:** `<SAV_MLFSAR_MIRROR_LINK>`

#### TinyVIRAT-v2-MLFSAR
- **Processed benchmark:** `<TINYVIRAT_MLFSAR_DOWNLOAD_LINK>`
- **Optional mirror:** `<TINYVIRAT_MLFSAR_MIRROR_LINK>`

> Replace the placeholder links above with your actual download URLs.

---

### 2. Split files

The fixed class splits used in our paper are provided in this repository:

```bash
data_splits/
├── SAV/
│   ├── base_classes.txt
│   ├── novel_classes.txt
│   ├── train_split.json
│   └── test_split.json
└── TinyVIRAT-v2/
    ├── base_classes.txt
    ├── novel_classes.txt
    ├── train_split.json
    └── test_split.json
