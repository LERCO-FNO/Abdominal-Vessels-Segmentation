# [BVM 2026] Automatic Deep Learning-Based Segmentation of Abdominal Vessels in CT Scans

## Overview

Welcome to the repository for the paper **"Automatic Deep Learning-Based Segmentation of Abdominal Vessels in CT Scans"**! 

### Read the paper:
(BVM 2026 proceedings – link coming soon)


Authors:  
Michal Nohel, Katerina Krejci, Constantin Ulrich, Maximilian Rokuss, Yannick Kirchhoff, Jiri Chmelik, Stefan Reguli, Jan Hrubovcak, Lubomir Martinek, Lukas Knybel 

Author Affiliations:  
Department of Deputy Director for Science and Research, University Hospital & Faculty of Medicine, Ostrava  
Department of Biomedical Engineering, Faculty of Electrical Engineering and Communication, Brno University of Technology, Brno, Czech Republic  
Division of Medical Image Computing, German Cancer Research Center (DKFZ), Heidelberg  
Medical Faculty Heidelberg, Heidelberg University  
Department of Neurosurgery, University Hospital & Faculty of Medicine, Ostrava  
Department of Surgery, University Hospital & Faculty of Medicine, Ostrava  
Department of Oncology, University Hospital & Faculty of Medicine, Ostrava  

## Overview
This repository contains the code and pre-trained models accompanying the paper submitted to BVM 2026, titled **"Automatic Deep Learning-Based Segmentation of Abdominal Vessels in CT Scans"**.  

The framework focuses on fully automatic 3D segmentation of abdominal arteries and veins from dual-phase contrast-enhanced CT (CECT) scans for preoperative surgical planning and vascular mapping.  

Our models are based on the nnU-Net ecosystem and were trained using three different trainer variants, including a topology-aware Skeleton Recall approach that improves segmentation continuity of thin tubular vessel structures and distal branches.


## Key Features

- **Artery Segmentation Network**: Automatic 3D segmentation of abdominal arteries from arterial phase CT scans  
- **Vein Segmentation Network**: Automatic 3D segmentation of abdominal veins from venous phase CT scans  
- Support for small distal vessels and third-order branching  
- Topology-aware training using Skeleton Recall loss  
- Five-fold cross-validation trained models  
- Standardized preprocessing and training pipeline based on nnU-Net

## Framework
The models were trained using the Skeleton Recall framework, which extends nnU-Net with a topology-aware loss function designed for tubular structure segmentation such as vessels.

Official repository:  
https://github.com/MIC-DKFZ/Skeleton-Recall

Skeleton-Recall internally builds on nnU-Net and provides compatible trainers (including SkeletonRecallTrainer) for training and inference.
 
## Usage
### Installation
Check out the official [nnUNet installation instructions](https://github.com/MIC-DKFZ/nnUNet/blob/master/documentation/installation_instructions.md)

It is **not necessary to install nnU-Net separately**.  
Installing Skeleton-Recall is sufficient, as it already includes the required nnU-Net components and dependencies.

Clone and install Skeleton-Recall:

```bash
git clone https://github.com/MIC-DKFZ/Skeleton-Recall.git
cd Skeleton-Recall
pip install -e .
```

After installation, the nnU-Net v2 commands (e.g., `nnUNetv2_predict_from_modelfolder`) will be available in your environment.

nnU-Net still needs to know where you intend to save raw data, preprocessed data and trained models.  
For this you need to set a few environment variables. Please follow the instructions here:  
https://github.com/MIC-DKFZ/nnUNet/blob/master/documentation/set_environment_variables.md

### Model Availability
The pre-trained models used in this study are publicly available and can be downloaded from:  
(GitHub / Zenodo link – to be added)

The released package contains trained models for:
- Artery segmentation (arterial phase CT)
- Vein segmentation (venous phase CT)

Each task includes three nnU-Net-based variants:
- Standard 3D nnU-Net  
- Residual Encoder U-Net (ResEnc)  
- SkeletonRecall nnU-Net (topology-aware)  

All models were trained using five-fold cross-validation on dual-phase CECT scans with manual annotations of abdominal vessels down to third-order branching.

# Inference
For inference you can use the default nnU-Net v2 inference functionalities provided through the Skeleton-Recall installation:  
https://github.com/MIC-DKFZ/nnUNet/blob/master/documentation/how_to_use_nnunet.md

## Artery Segmentation Network
For abdominal artery segmentation (arterial phase CT), run inference using the model folder:

```bash
nnUNetv2_predict_from_modelfolder -i INPUT_FOLDER -o OUTPUT_FOLDER -m ARTERY_MODEL_FOLDER
```



