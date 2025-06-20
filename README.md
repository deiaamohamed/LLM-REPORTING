# BioMedCLIP MRI Tumor Captioning — v1.2

**Version:** 1.2  
**Model Used:** `microsoft/BiomedCLIP-PubMedBERT_256-vit_base_patch16_224`  
**Framework:** OpenCLIP  
**Author:** Deiaa Mohamed

---

## Project Overview

This repository contains a pipeline that generates diagnostic-style captions for grayscale brain MRI images with red heatmaps indicating segmented tumor regions. The captions are selected based on similarity between the image and a curated list of clinical descriptions using BioMedCLIP.

---

## What Was Changed in v1.2

### 1. Rewriting and Expanding Caption Labels
- Replaced vague or repetitive labels (e.g., “a tumor is a mass that is a tumor”) with clearer, domain-specific phrasing.
- Split the label list into two logical groups:
  - Tumor-present cases (e.g., glioma, meningioma, pituitary)
  - Tumor-absent cases (e.g., healthy brain MRI without heatmaps)
- Added 8 new “no tumor” captions to make the classifier recognize clean scans.

### 2. Improving the Similarity Scoring
- Replaced raw similarity calculation with a normalized version:
  ```python
  confidence = (similarity_score + 1) / 2
