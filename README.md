# BioMedCLIP MRI Tumor Captioning — v1.2

**Version:** 1.2\
**Model Used:** `microsoft/BiomedCLIP-PubMedBERT_256-vit_base_patch16_224`\
**Framework:** OpenCLIP\
**Author:** Deiaa Mohamed

---

## Project Overview

This repository contains a pipeline that generates diagnostic-style captions for grayscale brain MRI images with overlaid red heatmaps indicating segmented tumor regions. The captions are selected based on similarity between the input image and a curated list of clinical descriptions using BioMedCLIP — a biomedical vision-language model.

The purpose is to improve explainability and diagnostic interpretation by pairing medical images with AI-generated captions and optional full diagnostic reports.

---

## What’s New in v1.2

### 1. Rewriting and Expanding Caption Labels

- Rewrote vague or redundant captions (e.g., “a tumor is a mass that is a tumor”) with clearer medical phrasing.
- Separated the label set into two categories:
  - Tumor-present (e.g., glioma, meningioma, pituitary tumor)
  - Tumor-absent (e.g., normal brain MRIs)
- Added 8 new "no tumor" labels to improve classifier performance on clean scans.

### 2. Normalized Similarity Scoring

- Switched from raw dot-product to a normalized confidence score:
  ```python
  confidence = (similarity_score + 1) / 2
  ```

### 3. LLM-Powered Report Generation

- Integrated with OpenRouter.ai to generate medical reports using:
  - Primary model: `mistral-7b-instruct`
  - Fallback model: `mistral-deverst-small` (used if the primary fails)

---

## How It Works

1. **Input:**\
   A grayscale brain MRI image overlaid with a red tumor segmentation heatmap.

2. **Encoding with BioMedCLIP:**\
   The image is encoded using BioMedCLIP’s visual encoder. A fixed set of medical captions is encoded with the text encoder.

3. **Caption Matching:**\
   The cosine similarity is calculated between the image and each caption. The caption with the highest normalized confidence score is selected.

4. **Optional Report Generation:**\
   The selected caption is sent to an API (OpenRouter.ai), which uses `mistral-7b-instruct` or `mistral-deverst-small` to generate a full diagnostic-style report.

---

### Example

**Input:**\
MRI scan of the brain with a segmented tumor in the left temporal lobe.

**System Output:**

- Caption:\
  `The MRI indicates presence of a glioma tumor in the temporal lobe.`
- Generated Report:\
  `This MRI scan reveals a glioma located in the temporal region, likely causing localized compression. Further evaluation with contrast-enhanced imaging is recommended.`

---

## Usage

### 1. Install Dependencies

```bash
pip install -r requirements.txt
```

### 2. Run Captioning

```bash
python caption_image.py --image_path ./data/sample_scan.png
```

### 3. Generate Full Report (Optional)

```bash
python caption_image.py --image_path ./data/sample_scan.png --generate_report
```

---

## Output Format

```
Image: ./data/sample_scan.png
Caption: The MRI indicates presence of a glioma tumor in the temporal lobe.
Confidence: 0.92

Generated Report:
This MRI scan reveals a glioma located in the temporal region, likely causing localized compression.
Further evaluation with contrast-enhanced imaging is recommended.
```

---

## Project Structure

```
BioMedCLIP-Captioning/
├── data/                  # Sample MRI scans
├── captions.txt           # Medical caption templates
├── models/                # BioMedCLIP wrappers
├── api/                   # Report generation API logic
├── caption_image.py       # Main entry script
├── utils.py               # Preprocessing and similarity functions
└── requirements.txt       # Required libraries
```

---

## Notes

- The pipeline works best with segmented images that clearly highlight tumor regions using red overlays.
- The generated caption is intended to aid interpretation, not replace expert diagnosis.
- Report generation is experimental and should be used with caution in real clinical workflows.

---

## Contact

**Deiaa Mohamed**\
Email: [[your\_email@example.com](mailto\:your_email@example.com)]\
GitHub: [[https://github.com/yourusername](https://github.com/yourusername)]

---

## Citation

If you use this work or build on it, please cite:

```bibtex
@misc{biomedclip2023,
  title={BioMedCLIP: Medical Vision-Language Pretraining},
  author={Microsoft Research},
  year={2023},
  url={https://huggingface.co/microsoft/BiomedCLIP-PubMedBERT_256-vit_base_patch16_224}
}
```

