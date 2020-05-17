# Setting up Python3 using Miniconda

The instructions here specifically assume a Linux x86_64 developer environment. Appropriate changes need to be made (e.g, compilers etc) for osx or power pc.

## Step 1:Miniconda3

If you do not have anaconda3 or miniconda3 setup on your machine, you will need to install it first. If are already a conda user, jump to Step 2.

- Download Miniconda3 from https://docs.conda.io/en/latest/miniconda.html
- Changer permissions on the downloaded script
```bash
chmod +x Miniconda3-latest-Linux-x86_64.sh
```
- Execute Miniconda3-latest-Linux-x86_64.sh
   - Pick an install location, e.g, /home/agram/python/miniconda3
```bash
./Miniconda3-latest-Linux-x86_64.sh
```

## Step 2: Set up a developer environment

We recommend setting us a new environment specifically meant for building and installing ISCE. If you intend to use the installed version with other packages, just install the other packages into the same environment at the very beginning. 

```bash
conda create -n nameofmyisceenv 
conda activate nameofmyisceenv
```

### Step 2.1 : Set up pre-requisites

The requirements file for the new environment is based on the recipe here:
https://github.com/conda-forge/isce2-feedstock

Like mentioned above, if you want other packages to be available in the same environment like jupyter, pandas etc - just add these to the requirements.txt file

You can decide on whether you want to use your system compilers and openmotif libraries (2.1a) or 
rely on conda for everything (2.1b) by setting up the requirements file appropriately. 

Once you have requirements.txt file, install the packages into your activated environment

```bash
conda install -c conda-forge --file requirements.txt
```

### Step 2.1 a - using system compilers and openmotif

```bash
> cat requirements.txt
python>=3.7
numpy
scipy
cython
scons
hdf5
h5py
gdal
libgdal
fftw
opencv=3.4.*
requests
```

### Step 2.1 b - using conda for everything

```bash
> cat requirements.txt
python>=3.7
numpy
scipy
cython
scons
hdf5
h5py
gdal
libgdal
fftw
opencv=3.4.*
openmotif
openmotif-dev
requests
xorg-libxt
xorg-libxft
xorg-libxmu
xorg-libxdmcp
gcc_linux-64
gxx_linux-64
gfortran_linux-64
```

### Step 2.2 : Create symlink for cython

In the activated environment, create a link for cython3 

```bash
ln -s ${CONDA_PREFIX}/bin/cython ${CONDA_PREFIX}/bin/cython3
```

--
**NOTE**

You should not hesitate to clean up your conda environments and recreate new ones as needed. Making changes to conda environments via activate.sh etc is probably a recipe for disaster. Get comfortable with setting up and dismantling conda environnments as needed. In general, updating an existing conda environment is also likely to result in incompatiblities between libraries. One is better off cleaning up and setting up a new environment from scratch to get newer versions of packages.
--
