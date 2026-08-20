# Biomedical Image-Analysis Pipeline — Nuclei Segmentation & Description

Student ID: 24142732

Hybrid pipeline combining a local multimodal LLM (via Ollama), classical image processing
(scikit-image), and a PyTorch U-Net, on the synthetic DAPI-like fluorescence-microscopy nuclei
dataset (`Assingnment-3-dataset`). **All 4 tasks + robustness extension complete, verified, and
executed end-to-end on Google Colab (T4 GPU). Report matches this exact notebook run — no
mismatch between report and code.**

## Files to submit to your teacher (only these 3)

| # | File | What it is |
|---|---|---|
| 1 | `Nimra_assignment_3.docx` | The 4-page report — figures, tables, prompts, real Q1–Q5 critical analysis, and references, all matching file #2 |
| 2 | `NIMRA_ASSIGNMENT_3.ipynb` | The executed notebook (every cell run, real outputs, 0 errors) — this is what the report was built from |
| 3 | `task4_hybrid_records.csv` | Task 4 deliverable: all 12 test images, JSON fields + real LLM narratives |

No other files are required for submission.

## Reference-only files (not required for submission)

`outputs/` (sample figures/model weights from an earlier verification run) and
`reference_scripts/` (standalone `.py` versions of each notebook section, plus an older local/VS
Code notebook variant) are kept here for your own reference only. Nothing in these two folders
needs to go to your teacher — everything in them is already inside file #2 above.

## Results summary (from the exact submitted run)

- **Task 1**: 80 train images, mean intensity 9.14 (std 18.15). Vision step used `llava` as an
  automatic fallback after `llama3.2-vision` failed to load ("unknown model architecture: mllama")
  — disclosed in the report. Two structured-prompt runs on the identical image returned different
  `notable_features` text and different `image_quality` values ("uncertain" vs "good").
- **Task 2**: Otsu + morphology + regionprops feature table; numbers-only summary sent to
  `llama3.2` (text model), which returned a paragraph + valid JSON without seeing the image.
- **Task 3**: U-Net trained from scratch, 15 epochs, 10.4s on Colab T4 GPU.
  Final val Dice **0.9942**, IoU **0.9885**. On the 12-image test split, U-Net beat Otsu on every
  image (mean Dice 0.9951 vs 0.9384; mean IoU 0.9902 vs 0.8841).
- **Task 4**: Hybrid pipeline run on all 12 test images with real per-image narratives from
  `llama3.2`; all 12 returned `quality_flag: reliable`.
- **Extension (robustness)**: low-contrast corruption collapsed the U-Net mask to a single
  16,383px blob (vs. 47.0px normally); blur degraded more gracefully (8→7 objects).
- **Report Section 5 (Q1–Q5)**: answered in full original prose (not bullet points), covering
  usefulness/trustworthiness of VLM vs numbers-first descriptions, U-Net vs Otsu comparison,
  Dice/IoU interpretation, LLM hallucination risks and mitigations, and clinical trustworthiness.

## Notes / limitations

- Trained/evaluated at 128x128 (not 256x256) to keep training fast on Colab's shared GPU quota.
- Dataset is fully synthetic (CC0) — Dice/IoU near 0.99 reflects a clean, noise-controlled set
  and should not be read as evidence of real-world clinical performance (see report Q5).
- Outputs for educational use only — not cleared for clinical use.
- If Ollama is re-run (e.g. by your teacher), VLM/LLM text outputs will differ slightly each time
  by design — this is expected non-determinism, not an error.
