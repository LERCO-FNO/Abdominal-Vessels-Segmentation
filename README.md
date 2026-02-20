# [BVM 2026] Automatic Deep Learning-Based Segmentation of Abdominal Vessels in CT Scans

## Overview

Welcome to the repository for the paper **"Automatic Deep Learning-Based Segmentation of Abdominal Vessels in CT Scans"**! 

### Read the [paper](BVM 2026 proceedings – link coming soon):

**Authors:** Michal Nohel<sup>1,2,†</sup>, Katerina Krejci<sup>2</sup>, Constantin Ulrich<sup>3,4</sup>, Maximilian Rokuss<sup>3,5,6</sup>, Yannick Kirchhoff<sup>3,5,6</sup>, Jiri Chmelik<sup>2</sup>, Stefan Reguli<sup>7</sup>, Jan Hrubovcak<sup>8</sup>, Lubomir Martinek<sup>8</sup>, Lukas Knybel<sup>9</sup>

**Author Affiliations:**  
<sup>1</sup> Department of Deputy Director for Science and Research, University Hospital & Faculty of Medicine, Ostrava, Czech Republic  
<sup>2</sup> Department of Biomedical Engineering, Faculty of Electrical Engineering and Communication, Brno University of Technology, Brno, Czech Republic  
<sup>3</sup> German Cancer Research Center (DKFZ), Division of Medical Image Computing, Heidelberg, Germany  
<sup>4</sup> Medical Faculty Heidelberg, University of Heidelberg, Heidelberg, Germany  
<sup>5</sup> HIDSS4Health - Helmholtz Information and Data Science School for Health, Karlsruhe/Heidelberg, Germany  
<sup>6</sup> Faculty of Mathematics and Computer Science, Heidelberg University, Heidelberg, Germany  
<sup>7</sup> Department of Neurosurgery, University Hospital & Faculty of Medicine, Ostrava, Czech Republic  
<sup>8</sup> Department of Surgery, University Hospital & Faculty of Medicine, Ostrava, Czech Republic  
<sup>9</sup> Department of Oncology, University Hospital & Faculty of Medicine, Ostrava, Czech Republic  
<sup>*</sup> Corresponding authors: Michal Nohel (michal.nohel@fno.cz)


## Overview
This repository contains the code and pre-trained models accompanying the paper submitted to BVM 2026, titled **"Automatic Deep Learning-Based Segmentation of Abdominal Vessels in CT Scans"**.  

The framework focuses on fully automatic 3D segmentation of abdominal arteries and veins from dual-phase contrast-enhanced CT (CECT) scans for preoperative surgical planning and vascular mapping.  

Our models are based on the nnU-Net ecosystem and were trained using three different trainer variants, including a topology-aware Skeleton Recall approach that improves segmentation continuity of thin tubular vessel structures and distal branches.

## Dataset
The models were trained on a dataset of **50 patients** with dual-phase CECT scans. The data format used for training is **NRRD**. The dataset is publicly available at Zenodo: [https://zenodo.org/records/17407158](https://zenodo.org/records/17407158).

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

All models were trained using five-fold cross-validation on the **50-patient dual-phase CECT dataset** with manual annotations of abdominal vessels down to third-order branching.


# Inference
For inference you can use the default nnU-Net v2 inference functionalities provided through the Skeleton-Recall installation:  
https://github.com/MIC-DKFZ/nnUNet/blob/master/documentation/how_to_use_nnunet.md

## Artery Segmentation Network
For abdominal artery segmentation (arterial phase CT), run inference using the model folder:

```bash
nnUNetv2_predict_from_modelfolder -i INPUT_FOLDER -o OUTPUT_FOLDER -m MODEL_FOLDER
```

This executes the trained model (or fold ensemble if available) for artery segmentation.

The provided models can be used in two ways:
- Five-fold cross-validation ensemble (recommended)
- A single model trained on all available data

To perform inference using a five-fold ensemble, use the following command:
```bash
nnUNetv2_predict_from_modelfolder -i INPUT_FOLDER -o OUTPUT_FOLDER -m MODEL_FOLDER
```

To perform inference using the model trained on all data, use:
```bash
nnUNetv2_predict_from_modelfolder -i INPUT_FOLDER -o OUTPUT_FOLDER -m MODEL_FOLDER -f all
```

## Vein Segmentation Network

For abdominal vein segmentation (venous phase CT), the models were trained using five-fold cross-validation.  
You can perform inference either using the five-fold ensemble or the model trained on all available data.

To perform inference using the five-fold ensemble, use:
```bash
nnUNetv2_predict_from_modelfolder -i INPUT_FOLDER -o OUTPUT_FOLDER -m MODEL_FOLDER
```

To perform inference using the model trained on all data, use:
```bash
nnUNetv2_predict_from_modelfolder -i INPUT_FOLDER -o OUTPUT_FOLDER -m MODEL_FOLDER -f all
```

## Recommended Model
Based on quantitative and qualitative evaluation presented in the paper, the SkeletonRecall model showed:
- Higher sensitivity for vessel detection  
- Better continuity of vascular trees  
- Improved segmentation of small and distal vessel branches  

Although global Dice scores can be slightly lower due to incomplete annotations of tiny vessels, the SkeletonRecall variant provides the most anatomically consistent and clinically useful results for preoperative planning and VR-based visualization pipelines.

## Citation
If you use this repository or the trained models in your research, please cite:
TO BE DONE





