# UMTRI_3DSegmentation
Tool for segmentation of most major anatomical structures in any CT or MR image. It was trained on a wide range of different CT and MR images (different scanners, institutions, protocols,...) and therefore works well on most images. A large part of the training dataset can be downloaded here: CT dataset (1228 subjects) and MR dataset (616 subjects). You can also try the tool online at totalsegmentator.com or as 3D Slicer extension.

# Installation
TotalSegmentator works on Ubuntu, Mac, and Windows and on CPU and GPU.

Install dependencies:

Python >= 3.9
Pytorch >= 2.0.0 and <2.6.0 (and <2.4 for windows)
Optionally:

if you use the option --preview you have to install xvfb (apt-get install xvfb) and fury (pip install fury)
Install Totalsegmentator
```python
pip install TotalSegmentator


Here is the instruction of how to run our basic segmentation model based on nnunet.

1. Setup nnU-Net as described here

2. Download the data
```python

3. Convert the data to nnU-Net format using resources/convert_dataset_to_nnunet.py (see resources/train_nnunet.sh for usage example)

4. Preprocess nnUNetv2_plan_and_preprocess -d <your_dataset_id> -pl ExperimentPlanner -c 3d_fullres -np 2
```python

5. Train nnUNetv2_train <your_dataset_id> 3d_fullres 0 -tr nnUNetTrainerNoMirroring (takes several days)

6. Predict test set nnUNetv2_predict -i path/to/imagesTs -o path/to/labelsTs_predicted -d <your_dataset_id> -c 3d_fullres -tr nnUNetTrainerNoMirroring --disable_tta -f 0

7. Evaluate python resources/evaluate.py path/to/labelsTs path/to/labelsTs_predicted (requires pip install git+https://github.com/google-deepmind/surface-distance.git and pip install p_tqdm). The resulting numbers should be similar to the ones in resources/evaluate_results.txt (since training is not deterministic the mean dice score across all classes can vary by up to one dice point)

8. Done
Note: This will not give you the same results as TotalSegmentator for two reasons:

TotalSegmentator v2 uses a bigger dataset which is not completely public
TotalSegmentator is trained on images without blurred faces. Your dataset contains blurred faces.
