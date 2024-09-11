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

#### Related Papers

In zotero


# MRI Analysis in Python (Notes)

Notes for materials from [the pybrain workshop](https://github.com/miykael/workshop_pybrain) and [this youtube video](https://www.youtube.com/watch?v=4FVGn8vodkc).
 

Two main packages used for analyzing MRI data in Python are [Nibabel](https://github.com/nipy/nibabel) and [Nilearn](https://nilearn.github.io/stable/index.html).
- Nibabel: saving and loading MRI data
- Nilearn: statistical learning with MRI data


### Using Nibabel to Inspect Images

The dataset used here is from the [brain dynamics during flow experience](https://www.kaggle.com/datasets/albertozorzetto/fmri-brain-dynamics-during-flow-experiences) data. Each subject has an anatomical MRI scan and functional MRI scans during 3 different tasks. 

The package `nibabel` can be used for loading and viewing MRI data. 

```python
# packages 
import nibabel as nb
import numpy as np
import matplotlib.pyplot as plt
```

### Anatomical MRI


##### Loading data & data structure

- The command `nb.load` can be used to input MRI image. The data file compatible with nibable usually ends with  `.nii` or `.nii.gz`.
- `get_fdata` gets the data array from an image file, the resulting data array's shape correspondes to the scan's dimension


In this case, because the scan is anatomical and not functional, the resulting data is 3-dimensional. The 3 dimensions correspond to the `i, j, k` axes in the voxel space. The **voxel space** refers to the coordinate space of a MRI image. This is different from a **scanner-subject reference space** which is relative to the scanner and uses the `x, y, z` as coordinates. In the reference space, the (0, 0, 0) coordinates of `x, y, z` is the magnet isocenter. Units for these are mms. Below, the data array shape is in voxel space. 


```python
# anatomical mri (3D because it is not functional)
img_anat = nb.load('data/sub-005/anat/sub-005_T1w_defaced.nii')
img_anat_data = img_anat.get_fdata()
print(img_anat_data.shape)
```

    (176, 240, 256)


Plot the center slice on all axes. From these plot, we can see that the `i` dimension is from left to right, `j` is from front to back, and `k` is top to bottom of the brain. 


```python

fig, axes = plt.subplots(1, 3)
axes[0].imshow(img_anat_data[img_anat_data.shape[0] // 2, :, :], cmap='gray', origin="lower")
axes[1].imshow(img_anat_data[:, img_anat_data.shape[1] // 2, :], cmap='gray', origin="lower")
axes[2].imshow(img_anat_data[:, :, img_anat_data.shape[2] // 2], cmap='gray', origin="lower")
center_voxel_pos = np.array(img_anat_data.shape) // 2
center_voxel = img_anat_data[center_voxel_pos[0], center_voxel_pos[1], center_voxel_pos[2]] 
print("position of voxel at center is i = %3d, j = %3d, k = %3d; voxel at center value: %5.2f" % (center_voxel_pos[0], center_voxel_pos[1], center_voxel_pos[2], center_voxel)) 

```

    position of voxel at center is i =  88, j = 120, k = 128; voxel at center value: 231.00



    
![](load_6_1.png)
    


##### From the voxel space to the reference space

The Affine matrix can be used to transform between the voxel and reference spaces. Essentially, the affine matrix is the function $f$ in $(x, y, z) =  f(i, j , k)$. 


```python
print(img_anat.affine) # the affine matrix 
print(img_anat.affine.shape) # shape of the affine matrix
```

    [[ 9.97948170e-01 -6.33936822e-02  9.20792390e-03 -8.05883331e+01]
     [ 6.38809726e-02  9.95547771e-01 -6.93098381e-02 -9.50348053e+01]
     [-4.77313204e-03  6.97556883e-02  9.97552693e-01 -1.17536789e+02]
     [ 0.00000000e+00  0.00000000e+00  0.00000000e+00  1.00000000e+00]]
    (4, 4)


Usually, the affine can be break down into a matrix $M$ and three components $a, b, c$ such that 
$$
\begin{bmatrix} x \\ y \\ z \end{bmatrix} = 
M \begin{bmatrix} i \\ j \\ k \end{bmatrix} + \begin{bmatrix} a \\ b \\ c \end{bmatrix} 
$$

In the following, using the above transformation is the same as using the `apply_affine` function in nibabel



```python
M = img_anat .affine[:3, :3]
abc = img_anat.affine[:3, 3]

# returns reference space given voxel space coordinates
def f(i, j, k):
    return M.dot([i, j, k]) + abc

print(f(center_voxel_pos[0], center_voxel_pos[1], center_voxel_pos[2]))
from nibabel.affines import apply_affine
print(apply_affine(img_anat.affine, center_voxel_pos))
```

    [ 0.80247819 21.18079358 18.10060273]
    [ 0.80247819 21.18079358 18.10060273]



```python
import nilearn.plotting
nilearn.plotting.plot_img(img_anat)
```

    /opt/anaconda3/lib/python3.9/site-packages/pandas/core/computation/expressions.py:21: UserWarning: Pandas requires version '2.8.4' or newer of 'numexpr' (version '2.7.3' currently installed).
      from pandas.core.computation.check import NUMEXPR_INSTALLED
    /opt/anaconda3/lib/python3.9/site-packages/pandas/core/arrays/masked.py:60: UserWarning: Pandas requires version '1.3.6' or newer of 'bottleneck' (version '1.3.2' currently installed).
      from pandas.core import (





    <nilearn.plotting.displays._slicers.OrthoSlicer at 0x7ff678d41bb0>




    
![](load_11_2.png)
    


### Functional MRI in three tasks


```python
img = nb.load('data/sub-005/func/sub-005_task-game_run-01_bold_defaced.nii')
img_data = img.get_fdata()
print(img_data.shape)
```

    (120, 120, 72, 185)


