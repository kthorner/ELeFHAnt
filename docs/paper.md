---
title: 'ELeFHAnt: Ensemble Learning for Harmonization and Annotation of Single Cell Data'
tags:
  - R
  - single cell
  - sequencing
  - annotation
  - classification
  - machine learning
authors:
  - name: Konrad Thorner
    index: 1
  - name: Praneet Chaturvedi
    index: 1 
affiliations:
 - name: Cincinnati Children’s Hospital Medical Center
   index: 1
date: 19 July 2021
bibliography: paper.bib
---

# Summary 

Single cell sequencing has become a powerful method for understanding biological systems at an increasingly granular level. For single cell RNA-seq data specifically, one of the primary questions is using the transcriptome to determine cell identity. It is common to visualize such data and find clusters of cells with similarity in gene expression, but assigning each cluster a cell type is a much more open-ended task. Taking advantage of publicly-available, annotated datasets in combination with supervised learning is a powerful method for addressing this question. Ensemble Learning for Harmonization and Annotation of Single Cells (ELeFHAnt) provides an easy to use R package for users to annotate clusters of single cells, harmonize labels across single cell datasets to generate a unified atlas, and infer relationships among cell types between two datasets. It provides users with the flexibility of choosing between random forest and SVM (Support Vector Machine) based classifiers or letting ELeFHAnt apply both in combination to make predictions. 

# Statement of Need

As an alternative to manual annotation, there are many automatic cell annotation tools currently available that employ either gene marker, correlation, or machine learning-based methods, each with varying levels of performance [@Pasquini:2021]. The label transfer functionality of Seurat is among the most well-known, but occurs on an individual cell level rather than a community level, often leading to over-annotation [@Stuart:2019]. There are also deep learning-based tools emerging such as scANVI, which utilizes generative models but requires significantly more computation time [@Xu:2021]. 

ELeFHAnt is a supervised machine learning-based tool that enables researchers to identify cell types in their scRNA-seq data while providing additional unique features. ELeFHAnt gives users the ability to use and compare not just one but multiple classification algorithms simultaneously through its ensemble method, weighting their predictions to produce the best consensus among them. In this ensemble, SVM and random forest are our two classifiers selected for their superior accuracy and computation time in a benchmarking study [@Abdelaal:2019]. Additionally, selecting the optimal reference is a challenge addressed by harmonization, that allows users to integrate multiple datasets together into an atlas. A standardized set of labels is generated across all of them, which can subsequently be used to annotate new datasets. Relationships between two datasets can also be deduced to better understand how each was annotated. This is provided in an easy to interpret heatmap format that compares all the cell types between them. Finally, a subsampling procedure is used to enable faster predictions while being shown not to influence reproducibility.

ELeFHAnt has been tested on multiple public datasets during development. It has also been used internally within Cincinnati Children’s Hospital Medical Center across multiple projects, including annotation of snRNA (single nucleus RNA) sequencing with a harmonized gut cell atlas. 

# Overview and Examples

ELEFHAnt makes use of Seurat for the initial input data and pre-processing. It will then generate the training and test sets from the reference and query respectively, with optional subsampling. SVM and Random Forest are the classifiers that can be used seperately or in an ensemble. Classification accuracy of both are used to assign weights to the predictions from each classifier. These weighted confusion matrices are normalized based on the largest number of cells shared among celltypes and assigned clusters. They are then added together for the final ensemble predictions.

Model
![Model diagram](https://github.com/praneet1988/ELeFHAnt/blob/main/ELeFHAnt.png)

Celltype Annotation

To demonstrate Celltype Annotation using ELeFHAnt we used Gut Cell Atlas(https://cellgeni.cog.sanger.ac.uk/gutcellatlas/fetal_RAWCOUNTS_cellxgene.h5ad) as reference and Fetal intestinal data (https://www.sciencedirect.com/science/article/pii/S0092867421005316) from Dr. Spence Lab as query. Reference and query were downsampled to 200 cells per Celltypes and seurat_clusters respectively for enabling fast computation.

![Reference](https://github.com/praneet1988/ELeFHAnt/blob/main/Examples/GutCell_Reference.png)
![Query](https://github.com/praneet1988/ELeFHAnt/blob/main/Examples/FetalIntestine_SpenceLab_Query.png)
![Output](https://github.com/praneet1988/ELeFHAnt/blob/main/Examples/CelltypeAnnotation_Example2.png)

Label Harmonization

To demonstrate LabelHarmonization we used three datasets: 1) Gut Cell Atlas(https://cellgeni.cog.sanger.ac.uk/gutcellatlas/fetal_RAWCOUNTS_cellxgene.h5ad) 2) Fetal intestinal data (https://www.sciencedirect.com/science/article/pii/S0092867421005316) from Dr. Spence's Lab 3) Fetal intestine data from STAR-FINDer (https://www.sciencedirect.com/science/article/pii/S009286742031686X). The attributes of each dataset are shown below as well as those of the integrated object. Data shown below is based on subsetting 200 cells per celltype in each dataset to harmonize the atlas of ~125k cells.

![Datasets used](https://github.com/praneet1988/ELeFHAnt/blob/main/Examples/gut_datasets.png)
![Before and after harmonization](https://github.com/praneet1988/ELeFHAnt/blob/main/Examples/HarmonizationExample_ELeFHAnt.png)

Deduce Relationship

To demonstrate Deduce Relationship we used two datasets that were also uses in the harmonization example: 1) Gut Cell Atlas(https://cellgeni.cog.sanger.ac.uk/gutcellatlas/fetal_RAWCOUNTS_cellxgene.h5ad) 2) Fetal intestinal data (https://www.sciencedirect.com/science/article/pii/S0092867421005316) from Dr. Spence's Lab. Data shown below is based on subsetting to 500 cells per celltype in each dataset.

![Celltype relationship heatmap](https://github.com/praneet1988/ELeFHAnt/blob/main/Examples/DeduceRelationship_Example.png)

# Acknowledgements

We would like to thank Drs. Emily Miraldi, Nathan Salomonis, Anil Jegga, and Aaron Zorn for their valuable feedback. 

# References