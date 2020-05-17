## Setting up ISCE 
-----------------------

This section describes the setting up of the ISCE software when you use anaconda for python.
The setup is general and allows one to simultaneously install multiple versions on the machine.


Lookup table for mapping variables to locations on disk. The variables are not environment variables. 
We just use these to simplify instructions.

|   VARIABLE   |   MY VALUE                        |     Comment                               |
|--------------|-----------------------------------|-------------------------------------------|
|    HOME      |  /home/agram                      |  Default home directory                   |
|    ROOT      |  /home/agram/tools/isce           |  ROOT folder for ISCE installation        |
|    MODDIR    |  /home/agram/privatemodules/isce |  Folder for storing module files for ISCE |
|    VERSION   |  201807                           |  Version number of ISCE release           |




###Step 1: Setup directory structure (Very first installation)
---------------------------------------------------------

This step only needs to be performed when you are installing ISCE (any version) for the first time on your machine. 
If you have an old version installed using these very set of instructions, you don't need to repeat Step 1.

1. Create the directory ROOT . This directory will contain all future ISCE installations.
```bash
> mkdir /home/agram/tools/isce
```

2. Create the following subdirectories under ROOT.
```bash
ROOT
|
|--src
|--build
|--install
|--config 
```

The commands for creating this directory structure
```bash
> cd /home/agram/tools/isce
> mkdir config src build install
```

3. Create the directory MODDIR. This needs to be done in folder HOME/privatemodules. Environment modules looks for user-defined module files in this location.
```bash
HOME
|
|--privatemodules
|  |
|  |--isce
```
The commands for creating this directory structure
```bash
> cd
> mkdir privatemodules
> mkdir privatemodules/isce
```


###Step 2: Download ISCE tarball/ clone repo
--------------------------------------------

Tarballs of different release versions of ISCE can be found [here](https://github.com/isce-framework/isce2/releases)

To generalize the instructions, we will refer to the downloaded tarball as "isce-VERSION.tar.gz".

At the time of writing these instructions, the latest release version was "v2.3.3".

Alternately, you can also clone the repository from [here](https://github.com/isce-framework/isce2).
When discussing the live git repository - we will use "latest" as the tag for VERSION.


###Step 3: Create subfolders for the new version
--------------------------------------------------

Create a new subfolder for the new version under each subfolder of ROOT.
```bash
ROOT
|
|--src
|  |--VERSION
|--build
|  |--VERSION
|--install
|  |--VERSION
|--config 
|  |--VERSION
```

Commands for setting up this directory structure
```bash
> cd /home/agram/tools/isce
> mkdir config/v2.3.3 src/v2.3.3 build/v2.3.3 install/v2.3.3
```

###Step 4: Untar tarball or clone repo in src/VERSION
-----------------------------------------------------

Untar the downloaded tarball in the src/VERSION folder. This will unpack a directory called isce. 

```bash
> cd /home/agram/tools/isce/src/v2.3.3
> tar xzvf ~/Downloads/isce-v2.3.3.tar.gz
```

If using the live git report
```bash
> cd /home/agram/tools/isce/src/latest
> git clone https://github.com/isce-framework/isce2
```

###Step 5: Setup modulefile for specific version of ISCE
---------------------------------------------------------

We use environment modules to activate/ deactivate a specific version of ISCE.
Every version will need its own module file located at MODDIR/VERSION, that sets up appropriate environment variables and paths.

Shown below is the template for isce/v2.3.3 located at /home/agram/privatemodules/isce/v2.3.3

```bash
#%Module1.0#####################################################################
##
## isce modulefile
##
## privatemodules/isce. Generated from dot.in by configure.
##
proc ModulesHelp { } {
        global version

        puts stderr "\tAdds ISCE directory to your PYTHONPATH environment variable"
        puts stderr "\n\tAlso sets up PATH and LD_LIBRARY_PATH variables\n"
        puts stderr "\n\tVersion $version\n"
}

module-whatis   "adds ISCE stuff to your environment"

# for Tcl script use only
set     version      3.2.10

#Change this for each version
set     isceversion "v2.3.3"

##Report conda env when being loaded
##To assist in debugging
if [ module-info mode load ] {
   set  cprefix [getenv CONDA_PREFIX]
   puts stderr "CONDA env being used: $cprefix\n"
}


set		basedir		/home/agram/tools/isce
set		installdir      $basedir/install/$isceversion/isce
setenv		ISCE_HOME	    $installdir
setenv		SCONS_CONFIG_DIR    $basedir/config/$isceversion

###To make apps available at command line
prepend-path	PATH	$installdir/applications

###To make mdx and other utils available at command line
prepend-path    PATH    $installdir/bin

###To make isce importable in python
prepend-path    PYTHONPATH    $basedir/install/$isceversion
```

Now, load this module using the command
```bash
> module load isce/v2.3.3
```

You should see that the activated conda environment is reported
```
CONDA env being used: /home/agram/miniconda3/envs/nameofmyisceenv
```

If this environment is not correct, unload the module, activate the correct conda environment and reload the module.

Once you have set this up, you should be able to see this module listed amongst the available modules:

```bash
> module list
  1) use.own        3) gee
  2) basic          4) isce/v2.3.3
```

If you don't see the isce module file listed, make sure you have loaded the "use.own" module
```bash
> module load use.own
```
This instructs modules to look for module files in your HOME/privatemodules folder.


###Step 6: Create SConfigISCE file in config/VERSION
----------------------------------------------------

SConfigISCE is the configuration file used to set paths to the correct directories for building ISCE. We will create a new SConfigISCE for each version to ensure the ability to modify every installed version as needed. The SConfigISCE file needs to be created under ROOT/config/VERSION folder.

```bash
> cd /home/agram/tools/isce/config/v2.3.3
> vi SConfigISCE
```

The template for SConfigISCE is shown below. This is based on https://github.com/conda-forge/isce2-feedstock/blob/master/recipe/build.sh . 

```bash

#Build Directory
PRJ_SCONS_BUILD = /home/agram/tools/isce/build/v2.3.3 

#Install Directory (must end with isce)
PRJ_SCONS_INSTALL = /home/agram/tools/isce/install/v2.3.3/isce

#Compilers - set depending on the set of compilers you are using
CC = gcc      #If using conda-compilers, use output of echo $CC in conda env
CXX = g++     #If using conda-compilers, use output of echo $CXX in conda env
FORTRAN = gfortran  #If using conda-compilers, use output of echo $FC in conda env

#Get CONDA_PREFIX value using echo $CONDA_PREFIX in your activated environment

#system lib folders not needed if using conda for everything
LIBPATH =   {CONDA_PREFIX}/lib /usr/lib64 /usr/lib

#system include folders not needed if using conda for everything
#PYTHON_INCDIR=`$PYTHON -c "from sysconfig import get_paths; print(get_paths()['include'])"`
#NUMPY_INCDIR=`$PYTHON -c "import numpy; print(numpy.get_include())"`
CPPPATH =  {CONDA_PREFIX}/include {PYTHON_INCDIR} {NUMPY_INCDIR} /usr/include

#system include path not needed if using conda for everything
FORTRANPATH =  {CONDA_PREFIX}/include /usr/include

#use {CONDA_PREFIX}/include and {CONDA_PREFIX}/lib if getting everything via conda
#system include and lib are only needed when using system motif and X11 packages
MOTIFLIBPATH = /usr/lib64              # path to libXm.dylib
X11LIBPATH = /usr/lib64                # path to libXt.dylib
MOTIFINCPATH = /usr/include          # path to location of the Xm directory with various include files (.h)
X11INCPATH = /usr/include            # path to location of the X11 directory with various include files

##If you want to enable cuda
##Make sure you have CUDA settings in your basic modulefile
ENABLE_CUDA = True
```



###Step 7: Install isce
----------------------------------------

The first thing to do to use any version of ISCE or install a version of ISCE is to make sure that the correct module file is loaded.
For this example, if my module file was not already loaded, I execute

```bash
module load isce/v2.3.3
```

Change to the source directory and use scons to install.
```bash
> cd /home/agram/tools/isce/src/v2.3.3/isce
> scons install
```

This should install isce. Run "scons install" twice to make sure that all components are installed successfully.


###Step 8: Ready to use
-----------------------
You are now ready to use isce. Load the modulefile for the version you want to use and when you want to restore the environment, unload the module

```bash
> module load isce/v2.3.3
> .... Your work ....
> module unload isce
```

To switch between versions, unload the module for the current version and load the module corresponding to the version you want to use. Modules is a clean way of managing your environment variables. 
