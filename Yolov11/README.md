# YOLO11 with Attention Mechanisms for Landslide Detection

This repository contains customized **Ultralytics YOLO11** object-detection architectures developed for **landslide detection** using remote-sensing imagery.

Three attention configurations are provided:

| Model | Attention Module | Attention Location |
|---|---|---|
| YOLO11 | SE, BAM, CAM, DA, GCM, SK, LCT, GuCT, SimAM, and SRM | Backbone + Neck |
| YOLO11 | SE, BAM, CAM, DA, GCM, SK, LCT, GuCT, SimAM, and SRM | Neck |
| YOLO11 | SE, BAM, CAM, DA, GCM, SK, LCT, GuCT, SimAM, and SRM | Backbone |

## Overview

The project investigates the integration of attention mechanisms into different stages of YOLO11 for landslide object detection. The customized architectures modify the standard YOLO11 feature-extraction and feature-fusion pipeline while retaining the YOLO11 detection head.

### Model configurations

**YOLO11 + Attention — Backbone + Neck**

Various attention modules are inserted after the backbone feature-extraction stage and at multiple feature-fusion stages in the neck.

**YOLO11 + Attention — Neck**

Various attention modules are integrated at multiple neck stages while the backbone retains the standard YOLO11 structure.

**YOLO11 + Attention — Backbone**

Various attention modules are inserted at the end of the backbone, while the neck retains the standard YOLO11 structure.

## Model Files

```text
models/
├── neck_backbone_attention/*.yaml
├── neck_attention/*.yaml
└── backbone_attention/*.yaml
```

The supplied YAML files define the custom architectures. They use the custom modules SE, BAM, CAM, DA, GCM, SK, LCT, GuCT, SimAM, and SRM.

## Architecture

The experimental configurations compare attention placement at different locations:

```text
                         YOLO11
                           |
          +----------------+----------------+
          |                |                |
          v                v                v
    Attention           Attention        Attention
   Backbone + Neck        Neck           Backbone
          |                |                |
          +----------------+----------------+
                           |
                           v
                 Landslide Detection
```

Place the detailed architecture figure at:


examples/architecture.png


## Dataset

The project is intended for **single-class landslide object detection** using the Ultralytics YOLO dataset format.

Example dataset YAML:

```yaml
path: ''
train: 'images/train'
val: 'images/valid'
test: 'images/test'

nc: 1
names:
  0: landslide
```

Expected directory structure:

```text
dataset/
├── images/
│   ├── train/
│   ├── valid/
│   └── test/
└── labels/
    ├── train/
    ├── valid/
    └── test/
```

The dataset itself is not included unless its license permits redistribution.

## Installation

Install Ultralytics and the required dependencies:

```bash
pip install ultralytics
```

or:

```bash
pip install -r requirements.txt
```

Verify the installation:

```bash
yolo checks
```

## Training

### YOLO11 + Attention

```bash
yolo train model=path_to.yaml data=dataset/landslide.yaml epochs=500 batch=8 imgsz=640
```

> Do not provide two `model=` arguments in the same command. When training from a customized architecture, specify the appropriate custom YAML through `model=`.

If you intend to transfer pretrained YOLO11 weights to a customized architecture, verify checkpoint compatibility before training.

## Validation

Example:

```bash
yolo val model=path_to/weights/best.pt data=dataset/landslide.yaml imgsz=640
```

You can use the same procedure for all attention models by changing the checkpoint path.

Recommended detection metrics include:

- Precision
- Recall
- mAP@50
- mAP@50–95

## Inference

Example:

```bash
yolo predict model=path/weights/best.pt source=examples/input imgsz=640 save=True
```

Representative prediction images can be placed in:

```text
examples/predictions/
```
## Repository Structure

```text
YOLO11-Attention-Landslide-Detection/
│
├── README.md
│
├── models/
│   ├── neck_backbone_attention/*.yaml
│   ├── neck_attention/*.yaml
│   └── backbone_attention/*.yaml
│
├── dataset/
│   ├── landslide.yaml
│── architecture.png


```

## Reproducibility

For reproducible experiments, record:

- Python version
- PyTorch version
- Ultralytics version
- CUDA version
- GPU model
- Dataset version
- Input image size
- Batch size
- Number of epochs
- Learning rate
- Optimizer
- Random seed
- Model configuration
- Attention configuration
- Training checkpoint

The modified source code used to register custom attention modules should be included whenever possible.

## Citation

### Ultralytics YOLO11

```bibtex
@software{yolo11_ultralytics,
  author = {Glenn Jocher and Jing Qiu},
  title = {Ultralytics YOLO11},
  version = {11.0.0},
  year = {2024},
  url = {https://github.com/ultralytics/ultralytics},
  license = {AGPL-3.0}
}
```

### Attention Modules

The attention implementations used as the basis for this work are associated with:

https://github.com/changzy00/pytorch-attention

Please cite the original publications corresponding to SKLayer, LCT, and SRM when reporting research results.

## Attribution

This project uses and modifies the **Ultralytics YOLO** framework:

https://github.com/ultralytics/ultralytics

The attention implementations are based on or adapted from:

https://github.com/changzy00/pytorch-attention

Please preserve the original copyright and license notices associated with these projects.

## License

Ultralytics states that its open-source YOLO software is available under the **AGPL-3.0** license, with an Enterprise licensing option for applicable use cases.

If this repository contains modified Ultralytics source code, review and follow the applicable licensing and source-distribution requirements before redistribution.

Official information:

https://docs.ultralytics.com/

https://docs.ultralytics.com/help/contributing/

## Acknowledgements

This work uses the Ultralytics YOLO framework and open-source attention implementations from the `pytorch-attention` repository.
