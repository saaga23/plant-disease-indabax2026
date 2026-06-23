# Plant Disease Detection - IndabaX Nigeria 2026

A reproducible training protocol for edge-based plant-disease classification on African smallholder crops.

## What it does

This repository contains a single Jupyter notebook that trains a lightweight image classifier to recognize crop diseases from leaf images. It is designed to run on CPU or edge hardware and targets reproducible research for IndabaX Nigeria 2026.

## How it works

- **Data preparation:** images are organized by crop and disease class. Exact duplicate images are removed with MD5 hashing before splitting.
- **Split:** stratified 70/20/10 train/validation/test split with a fixed seed so results can be reproduced.
- **Model:** MobileNetV2 pretrained on ImageNet, with a custom dense head.
- **Training:**
  - Phase 1 trains only the classification head for 10 epochs with the base frozen.
  - Phase 2 unfreezes the top 50% of the convolutional base and fine-tunes for up to 20 epochs at a lower learning rate.
  - An auto-fallback reverts to Phase 1 weights if Phase 2 does not improve validation accuracy by at least 0.5 percentage points.
- **Augmentation:** random flips, rotation, zoom, and brightness changes are applied only to the training set.

## Tech stack

- Python 3
- TensorFlow / Keras
- NumPy, pandas, scikit-learn
- Matplotlib, seaborn

## Results / Metrics

Held-out test-set accuracy after fine-tuning:

| Crop     | Baseline (ImageNet) | Final accuracy | Improvement |
|----------|--------------------:|---------------:|------------:|
| Tomato   | 34.69%              | 97.96%         | +63.27 pp   |
| Cassava  | 52.42%              | 98.39%         | +45.97 pp   |

The notebook also reports per-class precision, recall, and F1, and saves confusion matrices and a results CSV.

## How to run

1. Open `plant-disease.ipynb` in Kaggle or Jupyter.
2. Provide the dataset. On Kaggle, mount a dataset containing `cassava/` and `tomatoes_combined/` directories; locally, place the data in the expected root path or update `DATA_ROOT`.
3. Run all cells. The notebook will create an output directory with model weights, plots, and a zipped results package.

## Project status

Research notebook prepared for IndabaX Nigeria 2026.

## License

Intended license: CC BY 4.0 (a `LICENSE` file is not yet present in this repository).

---

## Suggested GitHub metadata

**Suggested descriptions (<=160 chars):**

1. Reproducible MobileNetV2 training protocol for plant-disease classification on African smallholder crops.
2. Edge-ready plant-disease classifier for tomato and cassava using transfer learning.

**Suggested topic tags:**

`tensorflow`, `mobilenetv2`, `plant-disease`, `transfer-learning`, `indabax`
