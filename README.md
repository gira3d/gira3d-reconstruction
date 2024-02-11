# GIRA3D Reconstruction
This component of GIRA3D enables compact point cloud modeling for depth-intensity reconstruction. The point
cloud modeling is performed via Self-Organizing Gaussian Mixture Modeling (SOGMM) presented in:

```
[1] K. Goel, N. Michael, and W. Tabib, “Probabilistic Point Cloud Modeling via Self-Organizing Gaussian Mixture Models,” IEEE Robotics and Automation Letters, vol. 8, no. 5, pp. 2526–2533, May 2023, doi: 10.1109/LRA.2023.3256923.
```
```
@ARTICLE{goel_probabilistic_2023,
  title = {Probabilistic Point Cloud Modeling via Self-Organizing Gaussian Mixture Models},
  author = {Goel, Kshitij and Michael, Nathan and Tabib, Wennie},
  year = {2023},
  month = may,
  journal={IEEE Robotics and Automation Letters},
  volume = {8},
  number = {5},
  pages = {2526--2533},
  issn = {2377-3766},
  doi = {10.1109/LRA.2023.3256923},
  keywords = {Adaptation models,Complexity theory,Computational modeling,Data models,field robots,Mapping,Point cloud compression,Probabilistic logic,RGB-D perception,Three-dimensional displays}
}
```
A video demonstrating the features of SOGMM can be found [here](https://youtu.be/v0DfhK1lyno).

If you use this codebase in your work, please consider citing the article above.

## Overview
This repository is a meta workspace containing two [`colcon`](https://colcon.readthedocs.io/en/released/) workspaces:

1. The `dry` workspace contains dependencies that we choose to compile from their source code.

2. The `wet` workspace contains the main codebase.

The `dry` workspace contains the following `git` submodules:

1. `eigen_colcon`: Colcon package to pull [Eigen 3.4](https://eigen.tuxfamily.org/index.php?title=3.4).

2. `pybind11_colcon`: Colcon package to pull [pybind11](https://pybind11.readthedocs.io/en/stable/).

3. `open3d_colcon`: Colcon package to pull [our modifications](https://github.com/rislab/Open3D/tree/feature/cuda-12) to [Open3D](http://www.open3d.org/).
> **Note**
> `open3d_colcon` will not be compiled for macOS.

The `wet` workspace contains the following `git` submodules:

1. `self_organizing_gmm`: C++ codebase accelerated with OpenMP for Intel/AMD/ARM CPUs.

2. `sogmm_open3d`: C++ codebase accelerated with CUDA and Open3D for Intel/AMD/ARM CPUs and NVIDIA GPUs.
> **Note**
> `sogmm_open3d` will not be compiled for macOS.

3. `sogmm_py`: Scripts for visualization, comparison, testing, and profiling.

## Supported Platforms
The codebase has been tested on the following platforms:

1. **Linux**

| Platform ID | OS | CPU | GPU | Memory (CPU/GPU) | CUDA Version|
| ----------- | ------------- | --- | --- | ---------------- | ---- |
| Ryzen/RTX3090 | Ubuntu 20.04 | AMD Ryzen Threadripper 3960X, 48 threads | NVIDIA GeForce RTX 3090 | 252 GB/24 GB | <= 12.1 |
| Intel/RTX3060 | Ubuntu 20.04 | Intel Core i9-10900K, 20 threads | NVIDIA GeForce RTX 3060 | 32 GB/12 GB | <= 12.1 |
| ARM-12c/Orin | Ubuntu 20.04 | ARMv8 Processor rev 1 (v8l), 12 threads | NVIDIA Jetson Orin | 32 GB | <= 11.4 |
| ARM-8c/Xavier | Ubuntu 20.04 | ARMv8 Processor rev 0 (v8l), 8 threads | NVIDIA Jetson Xavier | 16 GB | <= 11.4 |
| ARM-6c/TX2 | Ubuntu 20.04 | ARM Cortex-A57, 6 threads | NVIDIA Jetson TX2 | 8 GB | <= 10.1 |

2. **macOS**
> **Note**
> CUDA-dependent repository, `sogmm_open3d`, is not supported for macOS.

| Platform ID | OS | CPU | GPU | Memory (CPU/GPU) | CUDA Version|
| ----------- | ------------- | --- | --- | ---------------- | ---- |
| MacBook/Intel | MacOS Ventura 13.3.1 | 2.3 GHz 8-Core Intel Core i9 | N/A | 16 GB | N/A |

## Build and Install
We require using a Python 3.8 virtual environment using `pip`. Assume that the
virtual environment is created within this repository using the folder name
`.venv`. First, we set up the `colcon` build environment.

```bash
git clone git@github.com:gira3d/gira3d-reconstruction.git
git checkout master # or your favorite branch
git submodule update --recursive --init
cd gira3d-reconstruction
python3.8 -m venv .venv
source .venv/bin/activate
pip install --upgrade pip
pip install colcon-common-extensions wheel
```

For Python scripts, run-time dependencies can be installed using `pip`. Make sure
the virtual environment created above is active.
```bash
pip install numpy scikit-learn opencv-python scikit-image future matplotlib termcolor
```

Finally, run the `build` script:
```bash
./build
```

Some helper scripts are provided as follows:
1. `clean`: Clean the builds for both `dry` and `wet` workspaces.
2. `configure`: Pull the submodules.

> **Warning**
> If you run into issues with OpenMP on MacOS, you might need to use the `llvm` clang
> compiler as opposed to the Apple Clang compiler.

## Environment Setup
These steps also compile the python bindings. To use these python interfaces, first run:

```bash
workon
```
> **Note**
> For NVIDIA Jetson platforms (i.e., Orin, Xavier, and TX2), run `workon_jetson`.

### User Guide
```bash
pip install mkdocs mkdocs-material
mkdocs serve
```
You should be able to view the documentation at this level over at [localhost](http://127.0.0.1:8000/).

Utilize this documentation for tutorials and examples that use one or many of the submodules.

Note: The documentation is stored using git lfs. You may need to run the
following in order to make sure it is pulled correctly:

```
cd docs
git lfs install
git lfs pull
```

## Authors
Kshitij Goel, Wennie Tabib

## Acknowledgments
1. [`liegroups`](https://github.com/utiasSTARS/liegroups): The MIT License, Copyright (c) 2017 Lee Clement.
2. [`Open3D`](https://github.com/isl-org/Open3D): The MIT License (MIT), Copyright (c) 2018-2023 www.open3d.org.
3. [`Eigen`](https://gitlab.com/libeigen/eigen): Mozilla Public License, v. 2.0.
4. `CUDA`, `cuBLAS`, `cuSOLVER`: Copyright (c) 2022-2023, NVIDIA CORPORATION.
