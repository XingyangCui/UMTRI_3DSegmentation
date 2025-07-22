# UMTRI_3DSegmentation

Tool for segmentation of most major anatomical structures in any CT or MR image.  
It was trained on a wide range of different CT and MR images (different scanners, institutions, protocols,...)  
and therefore works well on most images.

A large part of the training dataset can be downloaded here:  
- CT dataset (1228 subjects)  
- MR dataset (616 subjects)

You can also try the tool online at [totalsegmentator.com](https://www.totalsegmentator.com) or as a 3D Slicer extension.

---

## 🛠️ Installation

TotalSegmentator works on Ubuntu, Mac, and Windows, and supports both CPU and GPU.

### ✅ Install dependencies:

- Python >= 3.9  
- PyTorch >= 2.0.0 and < 2.6.0 (`< 2.4` for Windows)

**Optional:**  
If you use the option `--preview`, install:
```bash
sudo apt-get install xvfb
pip install fury
```



Here is the instruction of how to run our basic segmentation model based on nnunet.

1. Setup nnU-Net as described here

### Step 2: Download the Data

You can download the raw data from the following link:

👉 [Download Raw Data](https://armis2.arc-ts.umich.edu/pun/sys/dashboard/files/fs//nfs/turbo/coe-mreedsensitive/Processing/Foot_and_Ankle/SK/Raw_Data)

> Note: Make sure you have access to the UMich Turbo storage system.


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
