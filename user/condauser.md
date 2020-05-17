# Using isce2 conda package

An isce2 conda-forge package has been available since May 2019. Using this pre-built package is the easiest way to install ISCE on your machine, especially for end users who are not interested in making changes to the source code. 

Starting from isce versions of 2.3.3, autoRIFT is pre-packaged and installed as part of isce. Starting from version 2.3.3, Python3.8 versions are also available.


## Installing isce2 via conda

### Step 1: Install anaconda / miniconda 

If you are not a conda user, see [here](../dev/anaconda.md) for instructions on installing conda / miniconda.

### Step 2: Create a new user environment

We recommend creating a new environment instead of installing all software you need in the base environment


```bash
conda create -n nameofmyisceenv 
conda activate nameofmyisceenv
conda install -c conda-forge isce2
```

If you happen to be installing isce2 along with other packages like jupyter, pandas etc - you will have to add an entry for isce2 in your requirements.txt or environment.yml file.

--
**NOTE**

You should not hesitate to clean up your conda environments and recreate new ones as needed. Making changes to conda environments via activate.sh etc is probably a recipe for disaster. Get comfortable with setting up and dismantling conda environnments as needed. In general, updating an existing conda environment is also likely to result in incompatiblities between libraries. One is better off cleaning up and setting up a new environment from scratch to get newer versions of packages.
--

### Step 3: Setting up environment variables

isce2 evolved like traditional prototyping software and hence relied on setting of PYTHONPATH and PATH variables. Hence, users still have to set some environment variables to work with the conda package. Unfortunately, there is no sure shot way of getting around this. 

When the "nameofmyisceenv" environment is activated, two environment variables are already available to users:

1. ISCE\_HOME - physical location of install directory
2. ISCE\_STACK - physical location of contributed stack processors


```bash
export PATH=$PATH:$ISCE_HOME/bin:$ISCE_HOME/applications
```

Users have to manage the path settings to the stack processor of choice on their own.


## Maintaing/ updating the conda package

The isce2 package is built using the recipe found here:
https://github.com/conda-forge/isce2-feedstock

There are known limitations of the pre-built conda package:

1. GPU support is not available
2. Power PC version is not available
3. mdx is not available on osx due to limitations of x11 packages on conda-forge
