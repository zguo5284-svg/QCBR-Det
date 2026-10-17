# Third-party attribution

## FT-FSOD and HED

The retained source snapshot is from **FT-FSOD**:

- Repository: https://github.com/Intellindust-AI-Lab/FT-FSOD
- Retained checkout commit: `c68cea00fac282ec526d59bca55f27b3203daf37`
- Actual snapshot file hashes: `provenance/ft_fsod_snapshot.json`
- License: Apache-2.0; see `third_party/FT-FSOD/LICENSE`.
- Paper: Xuanlong Yu, Youyang Sha, Longfei Liu, Xi Shen, and Di Yang. *A Closer Look at Cross-Domain Few-Shot Object Detection: Fine-Tuning Matters and Parallel Decoder Helps*. CVPR 2026. https://arxiv.org/abs/2603.28182

HED and the progressive adaptation foundation are credited to this work. QCBR-Det builds on them; it does not claim HED or MMGroundingDINO as newly invented modules. The snapshot includes modified MMDetection/OpenMMLab code, whose copyright notices remain intact. The actual retained files, rather than the upstream commit alone, identify the experiment dependency.

## OpenMMLab and model assets

MMDetection / MMGroundingDINO: https://github.com/open-mmlab/mmdetection

MMCV: https://github.com/open-mmlab/mmcv

MMEngine: https://github.com/open-mmlab/mmengine

The Swin-B detector checkpoint and BERT assets must be obtained separately under their upstream terms. Dataset access likewise follows the original providers' licenses. No third-party dataset imagery or font binaries is bundled.
