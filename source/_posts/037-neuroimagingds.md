---
title: Notes on Learning about Neuroimaging Data Analysis
date: 2024-09-08
updated: 2024-09-08
tags: [knowledge]
categories:  [tools]
description:  As someone who has little background in neuroscinece, I will attempt to learn how to analyzei neuroimaging data. 
hidden: false
cover: mri.jpg
top: true
---


# Resources

#### Tools
- [Nilearn](https://nilearn.github.io/stable/index.html): A Python library for ML of brain volumns
- [CONN Toolbox](https://web.conn-toolbox.org/)

#### Datasets
- [1000 Connectomes](https://fcon_1000.projects.nitrc.org/fcpClassic/FcpTable.html): public release of 1200+ ‘resting state’ functional MRI (R-fMRI) datasets independently collected at 33 sites
- [Cognitive Communication Science Lab fMRI data](https://www.kaggle.com/datasets/albertozorzetto/fmri-brain-dynamics-during-flow-experiences/data)
- [Emotion Recognition fMRI data](https://www.kaggle.com/datasets/irajahangari/fmri-dataset-for-emotion-recognition)

#### Tutorials
- [Analyzing MRI data in Python](https://www.youtube.com/watch?v=4FVGn8vodkc)
- [Andy's Brain Book](https://andysbrainbook.readthedocs.io/en/latest/index.html)
- [Andrew Jahn's youtube channel](https://www.youtube.com/@AndrewJahn): author of Andy's Brain Book
- [Functional Neuroimaging Analysis in Python](https://carpentries-incubator.github.io/SDC-BIDS-fMRI/index.html): web tutorial


# MRI Analysis in Python (Notes)

Notes for materials from [the pybrain workshop](https://github.com/miykael/workshop_pybrain) and [this youtube video](https://www.youtube.com/watch?v=4FVGn8vodkc).
 

Two main packages used for analyzing MRI data in Python are [Nibabel](https://github.com/nipy/nibabel) and [Nilearn](https://nilearn.github.io/stable/index.html).
- Nibabel: saving and loading MRI data
- Nilearn: statistical learning with MRI data

### Using Nibabel to Inspect Images

The pacakge Nibable can be loaded using `import nibabel as nb`

