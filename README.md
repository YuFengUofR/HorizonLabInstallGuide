# Install Guide

This document is for isntalling variable stuffs for CV and ML, etc. on research use. The machine I tested on is a Nvidia GPU 2080Ti. There are several tools I have tested on this GPU machine, I list them below:
- Caffe with Anaconda
- Vitual Environment, such as Pipenv and virtualenv
- Compile PyTorch from source on TX2

# Caffe [UNFINISHED]
Bofore installing caffe, we need to install Anaconda as prerequsite.
## Install Anaconda
For caffe, we need to first install [*Anaconda*](https://conda.io/docs/user-guide/install/index.html). First download the installer using the command below:
```
$ wget https://repo.continuum.io/archive/Anaconda2-2018.12-Linux-x86_64.sh
```
Then, install the package.
```
$ bash Anaconda2-2018.12-Linux-x86_64.sh
```
Follow the instructions to install. After finished the installation, it is optional to add the Anaconda directory into the path and `source` the `~/.bashrc` file.
```
$ source ~/.bashrc
```
Then, you can test whether you install *Anaconda* succcess by using:
```
$ conda -V
conda 4.5.12
```

To uninstall everything, use the command below to remove the directory
```
$ rm -rf anaconda2
```

[UNFINISHED] Because we can't sudo access on GPU machine, so currently, I don't know if there is a way to install caffe in user space.

## Install Caffe on TX2
There is a reference link to a [blog](https://jkjung-avt.github.io/caffe-on-tx2/) that shows how to install Caffe on TX2.

# Install Pipenv
Pipenv is a veritual environment to help with python package control for your own project. To install the pipenv, you can first enter the command below in your own user space:
```
$ pip install --user pipenv
```
After installed *pipenv* successfully, add the user base path into `PATH` in your own `~/.bashrc` file. To check the path using Python:
```
$ python -m site --user-base 
/[SOMEPATH]/.local
```
Then, add the path into your own `~/.bashrc` by adding this line below:
```
export PATH="$PATH:/[SOMEPATH]/.local/bin"
```
and then, enter `source ~/.bashrc` in your terminal.

To use pipenv is simple, simply install the package in your own directory.
```
$ pipenv install <YOUR_PACKAGE>
```

## How to use virtualenv

*virtualenv* can create a isolated virtual environment in current directory. For instance,
```
$ virtualenv my_dir
```
This will create a virtual environment for current directory. To activate the current virtual environment,
```
$ source my_dir/bin/activate
(my_dir) $  # Now, you are in virtual environment.
```
To deactivate the virtual environment, simply type:
```
(my_dir) $ deactivate
$   # Now, you are out.
```
Within the virtual environment, you can install anything you want to by simply use 
```
$ pip install <YOUR_PACKAGE>
```

## Run PyTorch with *virtualenv* on GPU machine.
In this case, I use a *virtualenv* to run a stereo vision network using pytorch.

First, clone a Stereo Vision DNN network repository.
```
$ git clone https://github.com/JiaRenChang/PSMNet.git
```
Before entering into *virtualenv*, first, check the CUDA version in order to install PyTorch. To check the CUDA verison:
```
$ /usr/local/cuda/bin/nvcc --version
nvcc: NVIDIA (R) Cuda compiler driver
Copyright (c) 2005-2017 NVIDIA Corporation
Built on Fri_Sep__1_21:08:03_CDT_2017
Cuda compilation tools, release 9.0, V9.0.176
```
In our case, the CUDA version is 9.0. And then, enter *virtualenv*
```
$ source my_dir/bin/activate
```
Then, within the virtual environment, we install pytorch and other dependancies.
```
$ pip install torch torchvision scikit-image
```
Now, you can run the actual DNN network:
```
$ python submission.py --maxdisp 192 --model stackhourglass --KITTI 2015 \
    --datapath ../KITTI_dataset/ --loadmodel pretrained_model_KITTI2015.tar
```
That's it. Don't forget to deactivate when you want to exit the virtual environment.



# Run Tensorflow with *virtualenv*
First, let's clone a repository of stereo vision DNN using tensorflow, in this case, we use GC-Net (Unofficial version).
```
$ git clone https://github.com/Jiankai-Sun/GC-Net.git
```
Enter the *virtualenv* and install related dependancies.
```
$ cd GC-Net
$ virtualenv .
$ source bin/activate       # now, we enter the virtual environment
$ pip install tensorflow scipy pillow       # all dependencies, maybe some more. 
$ CUDA_VISIBLE_DEVICES=n python main.py --gpu n --phase train --max_steps 50000 \ --learning_rate 0.00005 --output_dir $HOME/Applications/GC-Net/log/ --pretrain true
.... # now, you get a DNN running on tensorflow
$ deactivate
```


# Install PyTorch on Jetson TX2
In case of fail to install PyTorch via pip, there is the method to install PyTorch from source.
Frist, clone PyTorch repo.
```
$ cd ~
$ git clone https://github.com/pytorch/pytorch.git
```
Enter the directory and install prerequsites:
```
$ cd ./pytorch
$ git submodule update --init
```
Sometime you need to install or update the dependancies for PyTorch, then, enter below commands:
```
$ sudo pip install -U setuptools
$ sudo pip install -r requirement.txt
```
Sometimes, you don't have to, because those dependencies are already installed before.
After finished installing all the prerequisites, in case of CUDA 9.0 installed in your TX2, one solution to aviod an allocation error when running PyTorch is to change two files in the source code.
First, change the `CUDA_NUM_THREADS` in both files: `aten/src/ATen/cuda/detail/KernelUtils.h` and `aten/src/THCUNN/common.h`. Change the `CUDA_NUM_THREADS` to 
```
constexpr int CUDA_NUM_THREADS = 512;
```
Then, compile the source code using the command below:
```
$ sudo python setup.py install
```
If you see the compilation succeed, you should be able to run PyTorch on your TX2.
