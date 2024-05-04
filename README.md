# Pytorch in Houdini

I will put my PyTorch experiments inside Houdini on this repo.

## Pytorch 3d Installation

You need PyTorch3D working inside Houdini. It could be tricky; personally, I spent several hours making it work. However, I've never used a custom Python environment with Houdini before. I'll try to explain how to do this and the traps to avoid. I personally use Windows, but I think it's almost the same for Mac and Linux.

### Install CUDA
I used CUDA v11.8, but it was tricky. I had to override the **cub** because of some compiler errors while installing PyTorch3D. Maybe the newer version could help. I deleted the Cub inside the CUDA installation and replaced it with Cub 11.7 ([Reference Issue](https://github.com/facebookresearch/pytorch3d/issues/1227)) and modified the **CUB_VERSION** in **version.cuh** to make it think it's still the old version.

### Create a Python environment

It's very important that your Python environment uses the exact Python version as the Houdini you are using. Open a **Windows/Shell** and type **hython --version**. After, on an Anaconda prompt:
```
conda create --name _envname_ python=_pythonversion_
```
Activate your new environment:
```
conda activate _envname_
```

### Install the packages

Be careful, always use pip command to install packages, never use conda commands for this. Houdini allows the use of external environments, but it only uses the **sites** not the whole environment. Using conda command to install packages will make the environment not compatible with Houdini.

- Install PyTorch. I personally used this command: 
```
pip install torch==2.2.0 torchvision --index-url https://download.pytorch.org/whl/cu118
```
- Install PyTorch3D:
```
pip install "git+https://github.com/facebookresearch/pytorch3d.git@stable"
```
- Install the other packages I use: 
```
pip install matplotlib tensorflow tensorboard
```

### [Optional] Launch Tensorboard

```
tensorboard --logdir=tblogs/
```
The log needs to be the same as in the Houdini project. Be sure to be in the same folder when launching the command.
Open this url in your favorite browser **http://localhost:6006/**

### Create the environment variable

Create an environment variable called **PYTHONPATH**, it should be the **site-packages** inside the **Lib** folder of your Python path.

### Test

Open a Python shell **Windows/Python shell** and type **from pytorch3d.structures import Meshes**. It should freeze for several seconds. If no errors, congratulations, you are good to go.

## Anamorphosis

The meat of this repo. Generate an anamorphosis using input meshes and images provided as parameters.
![Turntable](https://github.com/xjorma/Pytorch3dHoudini/blob/main/Documentation/Dolphin.gif)

## MeshFit

It's almost just a port of the mesh fitting example from PyTorch 3D. [Reference](https://pytorch3d.org/tutorials/deform_source_mesh_to_target_mesh)
It's not impressive, but it's an interesting example to study.
