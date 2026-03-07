---
title: 'NVCC Master Guide: From Installation to Performance Tuning'
date: '2026-01-20T17:32:00+08:00'
# weight: 1
# aliases: ["/first"]
tags: ["NVCC"]
author: "PaperMoon"
# author: ["Me", "You"] # multiple authors
showToc: true
TocOpen: false
draft: false
hidemeta: false
comments: true
# description: "Desc Text."
canonicalURL: "https://canonical.url/to/page"
# disableHLJS: true # to disable highlightjs
disableShare: false
disableHLJS: false
hideSummary: false
searchHidden: false
hiddenInHomeList: true
ShowReadingTime: false
ShowBreadCrumbs: true
ShowPostNavLinks: true
ShowWordCount: true
ShowRssButtonInSectionTermList: true
UseHugoToc: true
math: true
cover:
    image: "<image path/url>" # image path/url
    alt: "<alt text>" # alt text
    caption: "<text>" # display caption under cover
    relative: false # when using page bundles set this to true
    hidden: true # only hide on current single page
editPost:
    URL: "https://github.com/MilknoCandy/milknocandy.github.io/tree/main/content"
    Text: "Suggest Changes" # edit text
    appendFilePath: true # to append file path to Edit link
---

## 1 Installation
### 1.1 Installation for python environment

To install `nvcc` inside a Python virtual environment, conda is all you need. It lets you pick the exact CUDA version for each env, so you never clash with system-wide installs. Please follow the steps below to install `nvcc`:
1. Search for all available `nvcc` version
```bash
# activate your conda environment first
$ conda activate xxenv
# search for available cudatoolkit versions
$ conda search cudatoolkit --channel conda-forge
# output like this:
Loading channels: done
# Name                       Version           Build  Channel             
cudatoolkit                   5.5rc1              p0  anaconda/pkgs/pro   
cudatoolkit                    5.5.1              p0  anaconda/pkgs/pro   
cudatoolkit                      6.0              p0  anaconda/pkgs/pro   
cudatoolkit                      7.0               1  anaconda/pkgs/pro   
cudatoolkit                      7.5               0  anaconda/pkgs/free  
cudatoolkit                      7.5               2  anaconda/pkgs/free  
cudatoolkit                      8.0               1  anaconda/pkgs/free  
cudatoolkit                      8.0               3  anaconda/pkgs/free  
cudatoolkit                      9.0      h13b8566_0  anaconda/pkgs/main  
cudatoolkit                      9.0      h13b8566_0  pkgs/main           
cudatoolkit                      9.2               0  anaconda/pkgs/main  
cudatoolkit                      9.2               0  pkgs/main    
...
```
2. Install specific version of `nvcc`
If you install nvcc within the base environment, you may run into errors such as `OSError: [Errno 39] Directory not empty: 'xxx/anaconda3/lib/ossl-modules'`. Therefore, I recommend installing it in a manually created virtual environment instead.
```bash
# Install specific version of `nvcc`
$ conda install -c nvidia cudatoolkit=11.8 -y # or conda install -c conda-forge cudatoolkit=x.x
```
Now let us check what we have obtained by running this.
```bash
$ conda list | grep cuda
# output like this:
cudatoolkit               11.8.0               h6a678d5_0    https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main
nvidia-cuda-cupti-cu11    11.8.87                  pypi_0    pypi
nvidia-cuda-nvrtc-cu11    11.8.89                  pypi_0    pypi
nvidia-cuda-runtime-cu11  11.8.89                  pypi_0    pypi
```
Oops, it looks like the `cuda-nvcc` package is missing. Let’s install it with conda.
```bash
$ conda search cuda-nvcc --channel nvidia
# output like this:
Loading channels: done
# Name                       Version           Build  Channel
cuda-nvcc                    11.5.50      h8f81028_0  nvidia              
cuda-nvcc                   11.5.119      h2e31d95_0  nvidia              
cuda-nvcc                    11.6.55      h5758ece_0  nvidia              
cuda-nvcc                   11.6.112      hf7fc535_0  nvidia              
cuda-nvcc                   11.6.124      hbba6d2d_0  nvidia              
cuda-nvcc                    11.7.64               0  nvidia              
cuda-nvcc                    11.7.99               0  nvidia              
cuda-nvcc                    11.8.89               0  nvidia              
cuda-nvcc                    12.0.76               0  nvidia      
...
# Install the corresponding version of nvcc
$ conda install -c nvidia cuda-nvcc=11.8.89 -y
```
Finally, let us check if `nvcc` is correctly installed.
```bash
$ nvcc --version
# output like this:
nvcc: NVIDIA (R) Cuda compiler driver
Copyright (c) 2005-2022 NVIDIA Corporation
Built on Wed_Sep_21_10:33:58_PDT_2022
Cuda compilation tools, release 11.8, V11.8.89
Build cuda_11.8.r11.8/compiler.31833905_0
```
Note that you may need to run the following command before checking `nvcc`.
```bash
# set temporary variable to system environment variable
export PATH=$CONDA_PREFIX/bin:$PATH
export CUDA_HOME=$CONDA_PREFIX
# or set this permanently
echo 'export PATH=/usr/local/cuda-12.1/bin:$PATH' >> ~/.bashrc
echo 'export LD_LIBRARY_PATH=/usr/local/cuda-12.1/lib64:$LD_LIBRARY_PATH' >> ~/.bashrc
source ~/.bashrc  # refresh
```
## 2 Basic Usage
Once `nvcc` is installed, you can start using it to compile your CUDA programs. Here are some basic commands to get you started:
1. Compile a CUDA program
```bash
$ nvcc -o my_program my_program.cu
```
This command compiles the `my_program.cu` file and generates an executable named `my_program`.

2. Compile with specific architecture
```bash
$ nvcc -arch=sm_61 -o my_program my_program.cu
```
This command compiles the CUDA program for a specific GPU architecture (in this case, `sm_61`).