[A Sentinel-2 multi-year, multi-country benchmark dataset for Semantic segmentation with deep learning]

### Description

Based on the Sen4AgriNet dataset, we produce two distinct sub-datasets of Sentinel-2 L1C images for experimentation:
- **Patches Assembled Dataset (PAD)**: all Sentinel-2 images expanding over 2 years (2019, 2020) and 2 regions (France, Catalonia) with pixel-wise labels for crop classification (overall 168 classes).
- **Object Aggregated Dataset (OAD)**: on top of PAD, the mean and std of each parcel is computed for each Sentinel-2 observation and used as input data. A single label for each parcel is used.

In this project I only used the Patches Assembled Dataset (PAD)

The above sub-dataset was downsized in order to be used for the semantic segmentation. Specifically, 5000 patches with 60-20-20 (train-val-test) split were sampled from the sub-dataset and the 11 most frequent classes were kept (*wheat*, *maize*, *sorghum*, *barley*, *rye*, *oats*, *grapes*, *rapeseed*, *sunflower*, *potatoes*, *peas*). The explored scenario is:

Scenario | Train | Test
--|---|---
1  | Catalonia (2019, 2020) | France (2019)

The input of the PAD model is the median of each month of observations from April through September.

### Requirements

This repository was tested on:
* Python 3.8
* CUDA 11.4
* PyTorch 1.11
* PyTorch Lightning 1.6

### The used model

[U-Net](https://link.springer.com/chapter/10.1007/978-3-319-24574-4_28)

#### COCO files

In the `coco_files/` folder are the COCO files required for training, validating and testing the model.

#### NetCDF4 files

In the `dataset/netcdf/` folder you should place the downloaded netCDF4 files.

#### Essential scripts

- `coco_data_split.py`: Uses the nectCDF4 data to produce three COCO files for training, validation and testing.
- `export_medians_multi.py`: Uses the netCDF4 data and the COCO files to compute the median image per month and export them to the disk.
- `compute_class_weights.py`: Computes the class weights based on the exported medians, to account for class imbalance.
- `pad_experiments.py`: The main script for training/testing the PAD models.
- `visualize_predictions.py`: Produces a visualization of the ground truth and the prediction of the given model for a given image.

#### Using the repo

**Preparation**
1. (Optional) Run `export_medians_multi.py` to precompute the medians needed for training, validation and testing.
2. If you don't want to use the given COCO files, then export your own using the `coco_data_split.py` script.

### Reported results

The results reported on the given COCO files are presented in the following tables.

#### PAD

Scenario | Model | Acc. W. (%) | F1 W. (%) | Precision W. (%)  
2  | U-Net | **83.12** | **57.85** | **61.57**
