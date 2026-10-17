# QCBR-Det

**Quality Calibration and Context--Boundary Refinement for Cross-Domain Few-Shot Object Detection**

Code, configurations and recorded experimental results for the fixed QCBR-Det method.

## Download the complete code and results

**[Download the complete QCBR-Det research release](QCBR-Det_public_release.zip)** (959 files, approximately 3.85 MiB compressed).

The ZIP preserves the complete directory structure, including the frozen algorithm code, the attributed FT-FSOD dependency snapshot, all recorded result JSONs, 18 derived CSV tables, figures, reproduction instructions and provenance manifests. Download and extract it before running the commands below. Standalone main-result, budget-control and component-ablation tables are also provided at the repository root for convenient inspection.

```bash
unzip QCBR-Det_public_release.zip
cd QCBR-Det
python tools/verify_results.py
python tools/export_tables.py
```

These verification commands use only the Python standard library and do not start training. Full GPU reproduction instructions are in `QCBR-Det/docs/REPRODUCE.md` after extraction. New release helpers were checked for syntax and CPU processing; no fresh GPU training was performed during packaging.

## Fixed algorithm

The foundation is **MMGroundingDINO-B / Swin-B**, with **HED from FT-FSOD**, plus three additions:

1. **HED**: the existing hybrid ensemble decoder, with one shared serial layer and five parallel layers.
2. **QCAL**: query/region localization-quality estimation and bounded detection-score adjustment.
3. **OCCA**: object--context feature comparison and residual class-score adjustment.
4. **SABR**: scale-aware boundary features and size-relative box refinement.

Stage I adapts the detector and QCAL for 100 epochs using progressive freezing/unfreezing. Stage II freezes this detector and QCAL and trains OCCA/SABR for 100 epochs. Quality/agreement/shot-conditioned gates, zero-initialized residual outputs and residual regularization control adaptation. Final-epoch checkpoints are used; there is no best-test-AP selection. HED and the progressive adaptation foundation are credited to FT-FSOD, not claimed as new contributions.

## Three-seed main results

COCO bbox AP@[0.50:0.05:0.95], in percent. Supports are fixed across training seeds 0, 1 and 2. The last column is mean ± sample SD of the three seed-wise six-domain averages; domain columns are three-seed means.

| Setting | ArTaxOr | Clipart1k | DIOR | DeepFish | NEU-DET | UODD | Average |
|---|---:|---:|---:|---:|---:|---:|---:|
| 1-shot | 36.68 | 56.89 | 20.00 | 40.04 | 10.67 | 21.70 | 31.00 ± 0.57 |
| 5-shot | 69.32 | 57.61 | 29.48 | 36.97 | 18.79 | 25.30 | 39.58 ± 0.41 |
| 10-shot | 74.28 | 52.60 | 30.96 | 44.66 | 22.60 | 30.48 | 42.60 ± 0.31 |

All 54 per-run main records are retained, without rounding before aggregation. Historical `FISH` and `clipart1k` names correspond to DeepFish and Clipart1k.

### HED-200 budget control

| Setting | HED-200 | QCBR-Det | Paired mean AP difference |
|---|---:|---:|---:|
| 1-shot | 31.03 ± 1.09 | 31.00 ± 0.57 | -0.03 |
| 5-shot | 39.49 ± 0.33 | 39.58 ± 0.41 | +0.09 |
| 10-shot | 40.54 ± 0.82 | 42.60 ± 0.31 | +2.06 |

The strongest controlled evidence is the 10-shot mean improvement. Low-shot means do not establish universal superiority. Equal nominal epochs do not imply equal compute or trainable capacity. Fixed-split seed variability is not support-resampling variability, and prior benchmark exposure remains a limitation. Cross-paper backbone, pretraining and support-protocol differences must be considered.

## Included evidence

- Main results: 54 QCBR-Det runs and 54 HED-200 control runs.
- Component ablation: all 16 combinations of HED/QCAL/OCCA/SABR at 10-shot, six domains, three seeds (288 evaluated records).
- Training-strategy ablation, exploratory support resampling, mechanism diagnostics, DIOR efficiency measurement, bounded inference sensitivity, error analysis and logged training-stability summaries.
- Frozen implementation/configuration hashes and result provenance. A documented supplementary audit-comparator repair is distinguished from the fixed main algorithm.

For the measured 20-class DIOR configuration the method adds 171,965 parameters; the general increment is `170665 + 65*C`, or 170,730--171,965 across the six domains. The recorded approximately 9.8% latency overhead is specific to the measured configuration.

## Reproduction and excluded assets

The archive contains `frozen/experiments/`, `third_party/FT-FSOD/`, `results/`, `figures/`, `tools/`, `environment/`, `provenance/` and `docs/`. The retained runtime was Linux/Python 3.9.25, PyTorch 2.1.0+cu121, MMCV 2.1.0, MMEngine 0.10.7 and four distributed ranks. See the archived environment inventory and detailed instructions rather than assuming compatibility with current dependency versions.

Datasets, annotations, pretrained/trained model weights, credentials, cloud environment binaries, manuscript sources and font binaries are **not** redistributed. Obtain assets from their original providers under their licenses. Historical checkpoint/support hashes identify provenance, not downloadable assets.

## Attribution and license

We acknowledge [FT-FSOD](https://github.com/Intellindust-AI-Lab/FT-FSOD), [MMDetection / MMGroundingDINO](https://github.com/open-mmlab/mmdetection), MMCV, MMEngine and their dependencies. See [THIRD_PARTY.md](THIRD_PARTY.md) and the archive's per-file notices. HED is adopted from:

Xuanlong Yu, Youyang Sha, Longfei Liu, Xi Shen, and Di Yang. *A Closer Look at Cross-Domain Few-Shot Object Detection: Fine-Tuning Matters and Parallel Decoder Helps*. CVPR 2026. [Paper](https://arxiv.org/abs/2603.28182).

Upstream Apache-2.0 terms and notices are retained. An explicit reuse license for newly authored QCBR-Det material has not yet been selected; see [LICENSE](LICENSE). No published acceptance or universal state-of-the-art claim is implied. Paper bibliographic details will be added when finalized.
