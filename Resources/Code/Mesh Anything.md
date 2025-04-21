---
tags:
  - software
  - utility
  - generator
created: 21.04.2025
up: "[[Code]]"
---
[https://github.com/buaacyw/MeshAnything](https://github.com/buaacyw/MeshAnything)

![[demo_video.gif]]

### **MeshAnything:  
Artist-Created Mesh Generation  
with Autoregressive Transformers**

[Yiwen Chen](https://buaacyw.github.io/)1,2*, [Tong He](https://tonghe90.github.io/)2†, [Di Huang](https://dihuang.me/)2, [Weicai Ye](https://ywcmaike.github.io/)2, [Sijin Chen](https://ch3cook-fdu.github.io/)3, [Jiaxiang Tang](https://me.kiui.moe/)4  
[Xin Chen](https://chenxin.tech/)5, [Zhongang Cai](https://caizhongang.github.io/)6, [Lei Yang](https://scholar.google.com.hk/citations?user=jZH2IPYAAAAJ&hl=en)6, [Gang Yu](https://www.skicyyu.org/)7, [Guosheng Lin](https://guosheng.github.io/)1†, [Chi Zhang](https://icoz69.github.io/)8†  
*Work done during a research internship at Shanghai AI Lab.  
†Corresponding authors.  
1S-Lab, Nanyang Technological University, 2Shanghai AI Lab,  
3Fudan University, 4Peking University, 5University of Chinese Academy of Sciences,  
6SenseTime Research, 7Stepfun, 8Westlake University

## Release
- [6/17] 🔥🔥 Try our newly released **[MeshAnything V2](https://github.com/buaacyw/MeshAnythingV2)**. Maximum face number is increased to **1600** in V2 with better performance.
- [6/17] We released the 350m version of **MeshAnything**.

## Contents
- [Release](https://github.com/buaacyw/MeshAnything#release)
- [Contents](https://github.com/buaacyw/MeshAnything#contents)
- [Installation](https://github.com/buaacyw/MeshAnything#installation)
- [Usage](https://github.com/buaacyw/MeshAnything#usage)
- [Important Notes](https://github.com/buaacyw/MeshAnything#important-notes)
- [TODO](https://github.com/buaacyw/MeshAnything#todo)
- [Acknowledgement](https://github.com/buaacyw/MeshAnything#acknowledgement)
- [Star History](https://github.com/buaacyw/MeshAnything#star-history)
- [BibTeX](https://github.com/buaacyw/MeshAnything#bibtex)

## Installation
Our environment has been tested on Ubuntu 22, CUDA 11.8 with A100, A800 and A6000.

1. Clone our repo and create conda environment

```
git clone https://github.com/buaacyw/MeshAnything.git && cd MeshAnything
conda create -n MeshAnything python==3.10.13 -y
conda activate MeshAnything
pip install torch==2.1.1 torchvision==0.16.1 torchaudio==2.1.1 --index-url https://download.pytorch.org/whl/cu118
pip install -r requirements.txt
pip install flash-attn --no-build-isolation
```

or

```shell
pip install git+https://github.com/buaacyw/MeshAnything.git
```

And directly use in your code as

```
import MeshAnything
```

## Usage
### Local Gradio Demo

```
python app.py
```

### Mesh Command line inference

```
# folder input
python main.py --input_dir examples --out_dir mesh_output --input_type mesh

# single file input
python main.py --input_path examples/wand.obj --out_dir mesh_output --input_type mesh

# Preprocess with Marching Cubes first
python main.py --input_dir examples --out_dir mesh_output --input_type mesh --mc
```

### Point Cloud Command line inference

```
# Note: if you want to use your own point cloud, please make sure the normal is included.
# The file format should be a .npy file with shape (N, 6), where N is the number of points. The first 3 columns are the coordinates, and the last 3 columns are the normal.

# inference for folder
python main.py --input_dir pc_examples --out_dir pc_output --input_type pc_normal

# inference for single file
python main.py --input_path pc_examples/mouse.npy --out_dir pc_output --input_type pc_normal
```

## Important Notes
- It takes about 7GB and 30s to generate a mesh on an A6000 GPU.
- The input mesh will be normalized to a unit bounding box. The up vector of the input mesh should be +Y for better results.
- Limited by computational resources, MeshAnything is trained on meshes with fewer than 800 faces and cannot generate meshes with more than 800 faces. The shape of the input mesh should be sharp enough; otherwise, it will be challenging to represent it with only 800 faces. Thus, feed-forward 3D generation methods may often produce bad results due to insufficient shape quality. We suggest using results from 3D reconstruction, scanning and SDS-based method (like [DreamCraft3D](https://github.com/deepseek-ai/DreamCraft3D)) as the input of MeshAnything.
- Please refer to [https://huggingface.co/spaces/Yiwen-ntu/MeshAnything/tree/main/examples](https://huggingface.co/spaces/Yiwen-ntu/MeshAnything/tree/main/examples) for more examples.