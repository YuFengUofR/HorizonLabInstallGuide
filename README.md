# HorizonLabInstallGuide
A guide to install some stuffs and how to play with pipenv and others.


# Install Guide

This document is for isntalling variable stuffs for CV and ML, etc. on research use. The machine I tested on is a Nvidia GPU 2080Ti. There are several tools I have tested on this GPU machine, I list them below:
- Caffe

# Caffe
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

## Install Pipenv
Pipenv is a veritual environment to help with python package control for your own project. To install the pipenv, you can first enter the command below in your own user space:
```
$ pip install --user pipenv
```

