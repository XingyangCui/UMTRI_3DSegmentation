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
