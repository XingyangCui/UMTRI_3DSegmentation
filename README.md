# 🧠 UMTRI_3D_Segmentation

A pipeline for segmentation of major anatomical structures in CT and MR images.  
This tool is trained on a wide variety of CT and MR scans from different institutions, scanners, and protocols, allowing it to generalize well to diverse inputs.

---

## 📂 Datasets

We use two key datasets in this project:

- 🦴 [Ribcage dataset (156 subjects)](https://www.dropbox.com/home/Xingyang%20Cui/TotalSegmentator_FineTuning)
- 🦶 [Foot & Ankle dataset (78 subjects)](https://armis2.arc-ts.umich.edu/pun/sys/dashboard/files/fs//nfs/turbo/coe-mreedsensitive/Processing/Foot_and_Ankle/SK/Raw_Data)

> ⚠️ *Note: The Foot & Ankle dataset is stored on UMich's ARC Turbo system. Please ensure you have access and are connected to the UMich network.*

---

## 🛠️ Installation Instructions

TotalSegmentator runs on **Ubuntu, Mac, and Windows**, and supports both **CPU** and **GPU** environments.

### ✅ Step 1: Set Up Python Environment

- Python >= 3.9  
- PyTorch >= 2.0.0 and < 2.6.0 (`< 2.4` for Windows)

Optional dependencies for preview rendering:

```bash
sudo apt-get install xvfb
pip install fury
```

### ✅ Step 2: Install TotalSegmentator
```bash
pip install TotalSegmentator
```
Here is the instruction of how to run our basic segmentation model based on nnunet.




### Step 3: Install Required CT Software(3D Slicer & MeshLab)
These tools are required for visualization and mesh editing:

🧰 3D Slicer – Open-source platform for medical image computing
[🔗 Download 3D Slicer](https://www.meshlab.net/)

🧱 MeshLab – Tool for editing and visualizing 3D meshes


[🔗 Download MeshLab](https://www.slicer.org/)


the DataConvert the data to nnU-Net format using resources/convert_dataset_to_nnunet.py (see resources/train_nnunet.sh for usage example)

4. Preprocess nnUNetv2_plan_and_preprocess -d <your_dataset_id> -pl ExperimentPlanner -c 3d_fullres -np 2
```python

5. Train nnUNetv2_train <your_dataset_id> 3d_fullres 0 -tr nnUNetTrainerNoMirroring (takes several days)

6. Predict test set nnUNetv2_predict -i path/to/imagesTs -o path/to/labelsTs_predicted -d <your_dataset_id> -c 3d_fullres -tr nnUNetTrainerNoMirroring --disable_tta -f 0

7. Evaluate python resources/evaluate.py path/to/labelsTs path/to/labelsTs_predicted (requires pip install git+https://github.com/google-deepmind/surface-distance.git and pip install p_tqdm). The resulting numbers should be similar to the ones in resources/evaluate_results.txt (since training is not deterministic the mean dice score across all classes can vary by up to one dice point)

8. Done
Note: This will not give you the same results as TotalSegmentator for two reasons:

TotalSegmentator v2 uses a bigger dataset which is not completely public
TotalSegmentator is trained on images without blurred faces. Your dataset contains blurred faces.
