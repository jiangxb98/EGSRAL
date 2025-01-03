# EGS-RAL：An Enhanced 3D Gaussian Splatting based Renderer with Automated Labeling for Large-Scale Driving Scene
This repository contains the official implementation associated with the paper "EGSRAL:An Enhanced 3D Gaussian Splatting Based Renderer with Automated Labeling for Large-Scale Driving Scene".

⭐ [Arxiv](https://arxiv.org/abs/2412.15550)

> Yixiong Huo\*, Guangfeng Jiang\*, Hongyang Wei, Ji Liu†, Song Zhang, Han Liu, Xingliang Huang, Mingjie Lu, Jinzhang Peng, Dong Li, Lu Tian, Emad Barsoum  
> AAAI 2025

![framework](images/egsral.png)

## ToDo

- [ ] The code is being approved internally.

## Dataset


In our paper, we use:

- KITTI City used in READ paper.
- NuScenes-S [164, 209, 359,916] used in S-NeRF paper.
- NuScenes-D [103 168 212 220 228 687] used in DrivingGaussian paper.

These datasets generated by the huo, yixiong.

- KITTI `dataset/KITTI`
- NuScenes-S `dataset/datasets_s`
- NuScenes-D `dataset/datasets_d`

> Note: If use the  NuScenes-D please use the python script `3dgs-utils/sort_rename_dgs.py` to generate the id map pkl file·

## Run

### Environment

> Baed on the [Deformable 3DGS](https://github.com/ingra14m/Deformable-3D-Gaussians).

```bash
pip install torch==2.1.0 torchvision==0.16.0 torchaudio==2.1.0 --index-url https://download.pytorch.org/whl/cu121
python -m pip install setuptools==69.5.1
pip install plyfile tqdm opencv-python 
cd submodules
pip install -e depth-diff-gaussian-rasterization
pip install -e simple-knn
```

### Train

#### **KITTI-City**

```bash
# train 3dgs
CUDA_VISIBLE_DEVICES=0 python train_kitti_3dgs.py -s dataset/KITTI -m logs/kitti_3dgs --data_device cuda --eval --port 6040
# render
CUDA_VISIBLE_DEVICES=0 python render_3dgs.py -m logs/kitti_3dgs --skip_train --data_device cuda --mode render --iteration 9999999 --scene_name kitti18
# metric
CUDA_VISIBLE_DEVICES=0 python metrics_alex.py -m logs/kitti_3dgs

# train deformable 3dgs
CUDA_VISIBLE_DEVICES=0 python train_kitti_d3dgs.py -s dataset/KITTI -m logs/kitti_d3dgs --data_device cuda --eval --port 6040
CUDA_VISIBLE_DEVICES=0 python render_d3dgs.py -m logs/kitti_d3dgs --skip_train --data_device cuda --mode render --iteration 9999999 --scene_name kitti18
CUDA_VISIBLE_DEVICES=0 python metrics_alex.py -m logs/kitti_d3dgs

# train EGSRAL without group
CUDA_VISIBLE_DEVICES=0 python train_kitti_egsral.py -s dataset/KITTI -m logs/kitti_egsral --data_device cuda --eval --port 6047
CUDA_VISIBLE_DEVICES=0 python render_egsral.py -m logs/kitti_egsral --skip_train --data_device cuda --mode render --iteration 9999999 --scene_name kitti18
CUDA_VISIBLE_DEVICES=0 python metrics_alex.py -m logs/kitti_egsral

# train EGSRAL with group
CUDA_VISIBLE_DEVICES=4,5,6,7 python train_kitti_egsral_group.py -s dataset/KITTI -m logs/kitti_egsral_group --data_device cuda --eval --port 6047
CUDA_VISIBLE_DEVICES=0 python render_egsral_group.py -m logs/kitti_egsral_group --skip_train --data_device cuda --mode render --iteration 9999999 --scene_name kitti18
CUDA_VISIBLE_DEVICES=0 python metrics_alex.py -m logs/kitti_egsral_group
```



## Experiment Results

### KITTI-City

<p align="center"><img src="images\kitti_city.png" width="50%" /></p>

### NuScenes-D

<p align="center"><img src="images\nuscenes_d.png" width="50%" /></p>

### NuScenes-S

<p align="center"><img src="images\nuscenes_s.png" width="40%" /></p>

## Visualization

<p align="center"><img src="images\vis_kitti.png" alt="framework" width="80%" /></p>

<p align="center">Qualitative comparison of novel view synthesis on the KITTI City dataset.</p>

<p align="center"><img src="images\auto_label.png" alt="framework" width="50%" /></p>

<p align="center">Visualizing 3D auto labeling on nuScenes.</p>

## Acknowledgments

We sincerely thank the authors of [3DGS](https://repo-sam.inria.fr/fungraph/3d-gaussian-splatting/) and [Deformable 3DGS](https://github.com/ingra14m/Deformable-3D-Gaussians), whose codes were used in our work.
