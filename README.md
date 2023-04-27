# PhySG: Inverse Rendering with Spherical Gaussians for Physics-based Relighting and Material Editing
* CVPR 2021. Project page: [https://kai-46.github.io/PhySG-website/](https://kai-46.github.io/PhySG-website/)

## Quick start
* Create conda environment
```bash
conda env create -f environment.yml
conda activate PhySG
```

* Download example data from [google drive](https://drive.google.com/drive/folders/16LJfHzR9DPVPSgCeF0_eEp5z22DHgOow?usp=sharing).

* Optimize for geometry and material given a set of posed images and object segmentation masks
```bash
cd code
python training/exp_runner.py --conf confs_sg/default.conf  --data_split_dir ../example_data/duck/train  --expname duck
```

* Render novel views, relighting and mesh extraction, etc.
```bash
cd code
# use same lighting as training
python evaluation/eval.py --conf confs_sg/default.conf \
                              --data_split_dir ../example_data/duck/test \
                              --expname duck \
                              --gamma 1.0 --resolution 512 --save_exr
```

Tips: for viewing exr images, you can use [tev hdr viewer](https://github.com/Tom94/tev/releases/tag/v1.17).

## Some important pointers
* ```code/model/sg_render.py```: core of the appearance modelling that evaluates rendering equation using spherical Gaussians.
	* ```code/model/sg_envmap_convention.png```: coordinate system convention for the envmap.
* ```code/model/sg_envmap_material.py```: optimizable parameters for the material part.
* ```code/model/implicit_differentiable_renderer.py```: optimizable parameters for the geometry part; it also contains our foward rendering code.
* ```code/training/idr_train.py```: SGD optimization of unknown geometry and material.
* ```code/evaluation/eval.py```: novel view rendering, relighting, mesh extraction, etc.
* ```code/envmaps/fit_envmap_with_sg.py```: represent an envmap with mixture of spherical Gaussians. We provide three envmaps represented by spherical Gaussians optimized via this script in the 'code/envmaps' folder.

## Prepare your own data
* Organize the images and masks in the same way as the provided data. 
* Make sure object of interest is inside the unit sphere by properly normalizing your camera parameters.
* As to camera parameters, we follow the same convention as [NeRF++](https://github.com/Kai-46/nerfplusplus) to use OpenCV conventions.

Acknowledgements: this codebase borrows a lot from the [awesome IDR work](https://github.com/lioryariv/idr); we thank the authors for releasing their code.


