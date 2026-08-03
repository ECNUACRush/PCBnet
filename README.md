# PCBnet
**PCBnet: A Dataset and Automatic Construction of SPICE Netlists from Schematic Images** 
Our paper is published on IEEE LAD 2026 (07.29-07.31 2026, Stanford, CA, USA)

PCBnet is a large-scale dataset and benchmark for PCB schematic understanding and automatic schematic-to-SPICE-netlist construction.

The project provides a topology-oriented pipeline that combines visual element recognition, circuit structure construction, and multi-agent correction to recover structured circuit connectivity from PCB schematic images.

> **Dataset status:** The data is on the way and will be released soon.

## Overview

Printed circuit board design remains highly dependent on manual engineering workflows. Although AI has shown considerable potential in electronic design automation, progress in PCB schematic understanding is limited by the lack of large-scale paired schematic–netlist datasets.

PCBnet is designed to address this gap. It contains real-world PCB schematic designs with detailed annotations for:

* Circuit components
* Component pins
* Wires
* Text regions
* Character-level text labels
* Paired SPICE netlists

The repository also contains an automatic image-to-netlist framework for recognizing schematic elements, recovering circuit topology, and correcting recognition errors using domain knowledge and multimodal agents.

## Dataset

PCBnet is constructed from more than 300 real-world PCB designs collected from open-source hardware projects and commonly used EDA formats, including KiCad and EasyEDA.

The dataset contains approximately:

| Annotation type     |                         Scale |
| ------------------- | ----------------------------: |
| PCB designs         |                          300+ |
| Component instances |                       50,000+ |
| Wire annotations    |                      150,000+ |
| Text regions        |                      100,000+ |
| Labeled characters  |                      400,000+ |
| SPICE netlists      | Paired with schematic designs |

Each design provides a one-to-one correspondence between the rendered schematic image and its structured circuit netlist.

The dataset is divided into training, validation, and test sets at the design level using an `8:1:1` split.

## Supported Tasks

PCBnet supports three primary tasks.

### Component Detection

Detect and classify circuit components from schematic images.

**Metric:** mean Average Precision, or mAP.

### Text Recognition

Recognize component identifiers, signal names, values, and other textual labels.

**Metrics:**

* Character Error Rate
* Word Error Rate
* Recognition accuracy

### Schematic-to-Netlist Construction

Recover component-pin connectivity and generate structured SPICE netlists from schematic images.

**Metric:** connectivity accuracy based on correctly recovered electrical connections.

## Method

The schematic-to-netlist pipeline consists of three stages.

### Stage 1: Visual Element Extraction

The pipeline extracts basic visual elements from the input schematic:

* Component detection using YOLO
* Text-region detection followed by PaddleOCR
* Wire segmentation using U-Net
* Wire skeletonization for topology reconstruction

### Stage 2: Circuit Structure Construction

The recognized elements are converted into a circuit graph through:

* Component-specific pin estimation
* Pin-to-wire connectivity inference
* Text-to-component matching
* Category-consistency constraints
* Geometric-alignment constraints
* Local-uniqueness constraints

The resulting circuit graph is subsequently converted into a structured SPICE netlist.

### Stage 3: Multi-Agent Correction

A multi-agent correction framework is used to resolve uncertain OCR predictions and structural ambiguities.

The correction process includes:

* Confidence-guided error filtering
* Knowledge-guided LLM correction
* Multimodal image-based correction
* Component-aware text pairing
* Retrieval-augmented PCB domain knowledge

## Main Results

| Task                      | Result |
| ------------------------- | -----: |
| Component detection mAP   | 94.54% |
| Wire extraction IoU       | 93.70% |
| Text recognition accuracy | 98.57% |
| Text recognition CER      |  1.43% |
| Text recognition WER      |  3.59% |
| Connectivity accuracy     | 84.47% |

These results demonstrate that explicit visual parsing and topology construction substantially outperform direct end-to-end inference with general-purpose multimodal language models on fine-grained PCB schematic understanding.

## Repository Structure

The repository will be organized as follows:

```text
PCBnet/
├── configs/                 # Training and evaluation configurations
├── data/                    # Dataset preparation and metadata
├── models/
│   ├── component_detection/
│   ├── text_recognition/
│   ├── wire_segmentation/
│   └── agent_correction/
├── netlist_construction/    # Pin estimation and topology reconstruction
├── scripts/                 # Training, evaluation, and preprocessing scripts
├── examples/                # Example schematic-to-netlist results
├── requirements.txt
└── README.md
```

The directory structure may be updated as the code and dataset are released.

## Installation

Clone the repository:

```bash
git clone https://github.com/<organization-or-user>/PCBnet.git
cd PCBnet
```

Create a Python environment:

```bash
conda create -n pcbnet python=3.10
conda activate pcbnet
pip install -r requirements.txt
```

Detailed installation instructions and model dependencies will be provided together with the code release.

## Dataset Preparation

The PCBnet dataset is currently being prepared for public release.

> **Data is on the way.**

After release, the dataset download links, directory structure, annotation format, and preprocessing instructions will be provided in this section.

The expected dataset structure is:

```text
data/
├── images/
├── components/
├── pins/
├── wires/
├── texts/
├── netlists/
└── splits/
```

## Usage

Training and inference commands will be added after the complete code release.

The intended workflow is:

```text
Schematic Image
      ↓
Component, Text, and Wire Recognition
      ↓
Pin Estimation and Connectivity Construction
      ↓
Multi-Agent Correction
      ↓
Structured SPICE Netlist
```

Example commands will follow this format:

```bash
python scripts/run_pipeline.py \
    --image examples/example_schematic.png \
    --output outputs/example_netlist.sp
```

## Citation

Please cite the following paper when using PCBnet or the schematic-to-netlist framework:

```bibtex
@article{huang2026pcbnet,
  title     = {PCBnet: A Dataset and Automatic Construction of SPICE Netlists from Schematic Images},
  author    = {Zhen Huang and Yuhao Gao and Yuzhi Liu and Daian Cheng and Chengyuan Shao and Yucheng Chen and Yongjian Jia and Futing Zhang and Yichen Shi and Wenhao Wang and Zuyan He and Yangbo Wei and Zhanfei Chen and Jinlong Yan and Yu Zhang and Haoying Wu and Ting-Jung Lin and Lei He},
  year      = {2026}
}
```

The BibTeX entry will be updated after the official publication information becomes available.

## License

The code and dataset licenses will be announced when the corresponding resources are released.

Please check the repository before using PCBnet for commercial applications or redistributing its data.

## Contact

For questions, suggestions, or collaboration inquiries, please contact:

**Zhen Huang**
Email: `zhenhuang@mail.ustc.edu.c`

Issues and pull requests are also welcome.
