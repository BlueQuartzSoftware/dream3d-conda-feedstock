# DREAM3D and Python (Anaconda Python) #

In order to use DREAM3D from within a Python environment you will need to pick one of two options:

+ 1: Install a prebuilt 'dream3d-conda' package
+ 2: Build DREAM.3D from source using the ananconda/condaforge environment


## Install a Prebuilt 'dream3d-conda' Package ##

See the instructions in the file python\_filter\_development.md

### Writing a DREAM3D Filter in Python ###

Please see the file _conda\_filter\_development.md_ file for instructions.

## Building a DREAM3D Anaconda Package ##

### Requirements ###

+ Install [Visual Studio](https://visualstudio.microsoft.com/downloads/) (Windows)
+ Install [Anaconda](https://docs.anaconda.com/anaconda/install/)
+ Setup [conda-forge](https://conda-forge.org/) channel (requires conda >= 4.6)

### Necessary Repositories ###

This repository is required. clone this repo.

```
git clone ssh://git@github.com/bluequartzsoftware/dream3d-conda-feedstock
```


### Anaconda Virtual Environment Setup ###

We are now ready to start building the DREAM3D Anaconda package.

+ Add the *conda-forge* channel and set it's priority:

```lang-console
(base) C:\Users\johnsmith> conda config --add channels conda-forge
(base) C:\Users\johnsmith> conda config --set channel_priority strict
```

+ Next we need to create an anaconda virtual environment where we will build and eventually install the **dream3d** package. In this example we are calling our virtual environment **d3d-build-37** and we are going to use python version 3.7. You should be able to use python versions 3.7/3.8/3.9. Create a new conda virtual environment that also has the *conda-build* package installed

```lang-console
(base) C:\Users\johnsmith> conda create -n d3d-build-37 python=3.7 conda-build
(base) C:\Users\johnsmith> conda activate d3d-build-37
(d3d-build-37) C:\Users\johnsmith> 
```

+ Use conda build DREAM3D within the **d3d-build-37** virtual environment:

The build takes around 20 min to complete on a 12 core machine with SSD storage.

The initial "solving environment" step may take a while in particular, and it does not have a progress indicator.

```lang-console
(d3d-build-37) C:\Users\johnsmith> cd dream3d-conda-feedstock
(d3d-build-37) C:\Users\johnsmith\dream3d-conda-feedstock> conda build .
```

Install locally built *DREAM3D* package into a different virtual environment

+ Make sure the Python version of the environment matches the version used to build DREAM3D.

```lang-console
(base) C:\Users\johnsmith> conda create -n d3d python=3.7
(base) C:\Users\johnsmith> conda activate d3d
(d3d) C:\Users\johnsmith> conda install -c /path/to/conda-bld dream3d-conda
```

## Running DREAM3D from Anaconda ##

DREAM3D, and related applications, must be run from within the conda environment.

i.e.:

```lang-console
(base) C:\Users\johnsmith> conda activate d3d-build-37
(d3d-build-37) C:\Users\johnsmith> DREAM3D.exe
(d3d-build-37) C:\Users\johnsmith> PipelineRunner --pipeline /path/to/pipeline.json
```

The same environment must also be used for the Python bindings. The Python bindings are within the dream3d package and can be accessed like the following:

```lang-python
from dream3d import simpl
from dream3d import simplpy

dca = simpl.DataContainerArray()
result = simplpy.create_data_container(dca, data_container_name='foo')
```
