## Setting up ISCE2 
-----------------------

#### Note: This set of notes has been updated to use cmake instead of scons. The last version of repo with scons instructions is commit "fca7ba24cf680658503a84e1487881bf77147b89".

This section describes the setting up of the ISCE software when you use anaconda for python.
The setup is general and allows one to simultaneously install multiple versions on the machine.


Lookup table for mapping variables to locations on disk. The variables are not environment variables. 
We just use these to simplify instructions.

|   VARIABLE   |   MY VALUE                        |     Comment                               |
|--------------|-----------------------------------|-------------------------------------------|
|    HOME      |  /home/agram                      |  Default home directory                   |
|    ROOT      |  /home/agram/tools/isce2          |  ROOT folder for ISCE installation        |
|    MODDIR    |  /home/agram/privatemodules/isce2 |  Folder for storing module files for ISCE |
|    VERSION   |  201807                           |  Version number of ISCE release           |




###Step 1: Setup directory structure (Very first installation)
---------------------------------------------------------

This step only needs to be performed when you are installing ISCE (any version) for the first time on your machine. 
If you have an old version installed using these very set of instructions, you don't need to repeat Step 1.

1. Create the directory ROOT . This directory will contain all future ISCE installations.
```bash
> mkdir /home/agram/tools/isce2
```

2. Create the following subdirectories under ROOT.
```bash
ROOT
|
|--src
|--build
|--install
```

The commands for creating this directory structure
```bash
> cd /home/agram/tools/isce2
> mkdir src build install
```

3. Create the directory MODDIR. This needs to be done in folder HOME/privatemodules. Environment modules looks for user-defined module files in this location.
```bash
HOME
|
|--privatemodules
|  |
|  |--isce2
```
The commands for creating this directory structure
```bash
> cd
> mkdir privatemodules
> mkdir privatemodules/isce2
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
```

Commands for setting up this directory structure
```bash
> cd /home/agram/tools/isce2
> mkdir src/latest build/latest install/latest
```

###Step 4: Untar tarball or clone repo in src/VERSION
-----------------------------------------------------

Untar the downloaded tarball in the src/VERSION folder. This will unpack a directory called isce. 

```bash
> cd /home/agram/tools/isce2/src/latest
> tar xzvf ~/Downloads/isce-latest.tar.gz
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

Shown below is the template for isce/latest located at /home/agram/privatemodules/isce2/latest

```bash
#%Module1.0#####################################################################
##
## isce2 modulefile
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
set     isceversion "latest"

##Report conda env when being loaded
##To assist in debugging
if [ module-info mode load ] {
   set  cprefix [getenv CONDA_PREFIX]
   puts stderr "CONDA env being used: $cprefix\n"
}


## Save conda env as variable for debugging later
set          condaprefix     [getenv CONDA_PREFIX]
setenv       ISCE_CONDA_PREFIX  $condaprefix

set		basedir		/home/agram/tools/isce2
set             srcdir          $basedir/src/$isceversion/isce2
set		installdir      $basedir/install/$isceversion
setenv		ISCE_HOME	    $installdir/packages/isce


###cmake command to be run from build/version
###modify this line if you want to use your own compilers
###other flags can be added here to control where cmake searches for packages
set-alias       cmakeisce       "cmake -DCMAKE_INSTALL_PREFIX=$installdir -DCMAKE_PREFIX_PATH=$condaprefix $srcdir"

###To make mdx and other apps available at command line
prepend-path    PATH    $installdir/bin

###Make shared libraries available
append-path     LD_LIBRARY_PATH $installdir/lib

###To make isce importable in python
prepend-path    PYTHONPATH    $installdir/packages

```

Now, load this module using the command
```bash
> module load isce/latest
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
  2) basic          4) isce2/latest
```

If you don't see the isce module file listed, make sure you have loaded the "use.own" module
```bash
> module load use.own
```
This instructs modules to look for module files in your HOME/privatemodules folder.


###Step 6: Install isce
----------------------------------------

The first thing to do to use any version of ISCE or install a version of ISCE is to make sure that the correct module file is loaded.
For this example, if my module file was not already loaded, I execute

```bash
module load isce2/latest
```

Change to the source directory and use cmake+make to install.
```bash
> cd /home/agram/tools/isce2/build/latest/
> cmakeisce
> make
> make install
```

###Step 7: Ready to use
-----------------------
You are now ready to use isce. Load the modulefile for the version you want to use and when you want to restore the environment, unload the module

```bash
> module load isce2/latest
> .... Your work ....
> module unload isce2
```

To switch between versions, unload the module for the current version and load the module corresponding to the version you want to use. Modules is a clean way of managing your environment variables. 
