TILseg README File

Last Updated: March 12th, 2023

## CONTENTS: ##
1. About
2. Methodology
3. Installation
4. Example
5. Example Use Case
7. References
8. Repo Structure


Need to discuss if we need these:
2. Preprocessing
3. Model Selection
4. Segmentation (Seg)
5. Cluster Processing


### 1. ABOUT: ###
- - - -
TILseg (Tumor-Infiltrating Lymphocyte segmentation) is a software created to segment different types of cells capatured on a whole slide image of breast tissue. The tissue is stained using hematoxylin and eosin (H&E), then the resulting images are often used by pathologists to diagnose breast cancer. Tumor-infiltrating lymphocytes (TILs) are often found in high concentrations in breast cancer tissue. Therefore, reliable identification of cell types, and their locations is imperative for accurate diagnoses. This software aims to complement the diangosis pipeline by automating the segmentation and quantification of these cells from whole slide images. Approaches, like TILseg, are carving out the interface between computational tools and traditional histological, pathological, and medicinal approaches. 

This software provides a straightforward method for analyzing H&E stained microscope images and classifying and quantifying TILs present in the stained image. Briefly, the software takes in a given number of slide images, divides the image into a set of smaller patches, and filters out patches that do not contain significant amounts of tissue. A superpatch consisting of several smaller patches, from multiple slide images, is then passed into the machine learning model. Hyper-parameters optimization with various clustering algorithms can be performed by the software. Once training and validation via scoring of clustering is complete, the model is used to identify different cell types and segment and enumerate identified TILs. Detailed images and comma-separated value files with quantifiable details are created for each patch containing tissue.

This software is broken into three different modules that users can call: `preprocessing.py`, `model_selection.py`, and `seg.py`. The modules are intended to be used sequentially and their main functions/use cases are outlined in sections below.

### METHODOLOGY: ###
- - - -
Ryan's flowchart

### INSTALLATION: ###
- - - -
#### Dependencies: ####
- [matplotlib](https://matplotlib.org) = 3.6.2
- [numpy](https://numpy.org/) = 1.22.3
- [opencv](https://opencv.org/) = 4.6.0
- [openslide](https://openslide.org/) = 3.4.1
- [openslide-python](https://openslide.org/api/python/) = 1.1.2
- [pandas](https://pandas.pydata.org/) = 1.5.2
- [pillow](https://pillow.readthedocs.io/en/stable/) = 9.3.0
- [python](https://www.python.org/) = 3.10.9
- [scikit-image](https://scikit-image.org/) = 0.19.3 
- [scikit-learn](https://scikit-learn.org/stable/) = 1.0.2
- [scipy](https://scipy.org/) = 1.7.3  
These dependencies can be most easily handled using the provided environment.yml file to create a conda virtual environment. To do this:  
1. Install [Anaconda](https://www.anaconda.com/). 
2. Clone this repository
    - For example by running the command `git clone git@github.com:TILseg/TILseg.git` 
3. From the TILseg directory run `conda env create -f environment.yml`
    - The environment can then be activated using `conda activate tilseg`


### EXAMPLE: ###
- - - -
Original H&E Patch
:-------------------------:
![Original](https://user-images.githubusercontent.com/121774063/224920422-fb696076-d907-45af-89ab-3f053dd89747.jpg)


Cluster Overlay               |  Cluster Mask
:-------------------------:|:-------------------------:
![Image3](https://user-images.githubusercontent.com/121774063/224920501-9a2b0f81-847a-4e08-8a60-e726f5e4d405.jpg)  |  ![Image3](https://user-images.githubusercontent.com/121774063/224920528-ef4b2c34-5695-46a7-b020-09dc4e068375.jpg)


All Clusters               |  Segmented TILs Overlay
:-------------------------:|:-------------------------:
![AllClusters](https://user-images.githubusercontent.com/121774063/224920465-6b5c79f6-6431-46cf-a16e-59fe66fdbc28.jpg)  |  ![ContourOverlay](https://user-images.githubusercontent.com/121774063/224920555-414d718b-6ce0-4920-9af0-01b1c6cc2b96.jpg)


### 2. PREPROCESSING: ###
- - - -
The preprocessing module is called using one function: `preprocess()`. The function has six arguments, only one of which is required. The high level functionality is as follows:
- **Input**. A file path that contains all slide image files that will be processed.
- **Output**. A numpy array containing information for the superpatch created, as well as all filtered tissue images saved to their respective directories. In addition, the percentage of pixels lost due to preprocessing is also printed. This should be significantly less than one percent, but is included for the user if it is needed.

More specifically, the arguments taken for the main function are outlined below:
- **path (required):** the path to which all slide image files will be found. A subdirectory within this directory will be created for each slide, and all tissue images (after filtering) will be saved to those folders. The folders will be named the same as the slide image title. The superpatch used for training will be saved in this directory as `superpatch_training.tif`.
    - Required file type for slide images: `.svs`, `.tif`, `.ndpi`, `.vms`, `.vmu`, `.scn`, `.mrxs`, `.tiff`, `.svslide`, `.bif`

- **patches (default=6):** the number of patches to include in the superpatch. A selection of this number of small patches across all slides will be made to best represent the diversity in slide images. The superpatch will then be used for training the model downstream.

- **training (default=True):** a boolean that describes if the preprocessing step is for new slides that will not be put through training (only for model usage) or if they should be used in training, and therefore if a superpatch should be output as a result. 
    - `True` indicates that a superpatch will be created
    - `False` indicates that no superpatch will be created, but filtered patches will still be saved

- **save_im (default=True):** a boolean that describes if the preprocessing step should also save all images after filtering out background, to a subdirectory within the original directory for each slide. For either case, the superpatch will be saved as an image and the numpy array will still be returned.
    - `True` indicates that all filtered images will be saved
    - `False` indicates that all filtered images will not be saved

- **max_tile_x (default=4000):** the maximum number of pixels in the x direction for a patch. The software will attempt to get as close to this number as possible when splitting up the slide image.

- **max_tile_y (default=3000):** the maximum number of pixels in the y direction for a patch. The software will attempt to get as close to this number as possible when splitting up the slide image.

The numpy array of the superpatch is ultimately returned which is then fed into the model selection and segmentation process that occurs in the following modules.

### 3. MODEL SELECTION: ###
- - - -


### 4. SEGMENTATION (SEG): ###
- - - -


### 5. CLUSTER PROCESSING: ###
- - - -


### 6. EXAMPLE USE CASE: ###
- - - -
Please reference the `tilseg_example.ipynb` file for an example of how this software may be used. The four primary modules above are implemented with a sample slide image for illustrative purposes.

### 7. REFERENCES: ###
- - - -
This work is picked up from [another project](https://github.com/ViditShah98/Digital_segmentation_BRCA_547_Capstone) that originated from the CHEM E 545/546 course at the University of Washington.

This work is a continuation of the research performed in [Mittal Research Lab](https://shachimittal.com/) at the University of Washington.

### REPO STRUCTURE ###
- - - -
```
TILseg
-----
tilseg/
|-__init__.py
|-preprocessing.py
|-model_selection.py
|-cluster_processing.py
|-seg.py
|-test/
||-__init__.py
||-test_preprocessing.py
||-test_model_selection.py
||-test_cluster_processing.py
||-test_seg.py
||-test_patches/
|||-test_img.txt
|||-test_patch.tif
|||-test_superpatch.tif
|||-patches/
||||-test_small_patch.tif
||||-test_small_patch_2.tif
docs/
|-COMPONENTS.md
|-USECASES.md
|-USERSTORIES.md
environment.yml
LICENSE
README.md
examples/
|-tilseg_example.ipynb
.gitignore
```
