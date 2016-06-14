###Environment modules
----------------

This section describes the setting up of "environment modules" on your old linux machine

####Option 1: Use yum / apt

Most linux distros include a version of modules (atleast now a days). 
```bash
> sudo yum install environment-modules

(or)

> sudo apt-get install environment-modules
```

If this doesn't work, use Option 2

#### Option2 :

We will build this from scratch.

1. Download modules from here: https://sourceforge.net/projects/modules/
2. Note that you will need the macports installation done before you start with modules.
3. In the untarred directory:
    - ./configure --prefix=/usr/local
    - make
    - sudo make install

4.Modify ~/.bashrc as shown below. If using another shell, adapt the commands as needed:
```bash
# .bashrc

# Source global definitions
if [ -f /etc/bashrc ]; then
	. /etc/bashrc
fi

# User specific aliases and functions

###Initialize modules
source /usr/share/Modules/init/bash
###Instruct to use modules from your home folder
module load use.own basic

###You can also load basic environment at startup (optional)
module load basic
```


### Setting up basic environment

We will create a default module file that will use your anaconda setup.

Here is a template for the basic module file. Please modify contents to suit your needs.
The main purpose of this basic module is to :

1. Use recently installed miniconda2 as your default python/ ipython
2. Use recently installed anaconda3 as your default python3/ ipython3
3. Not mangle your environment with unnecessary PATH and PYTHONPATH variables. These should be organized as their own modules.

```bash
> vi /home/agram/privatemodules/basic
```
Copy the template shown below and make edits as needed.
```bash
#%Module1.0#####################################################################
##
## dot modulefile
##
## modulefiles/dot.  Generated from dot.in by configure.
##
proc ModulesHelp { } {
        global dotversion

        puts stderr "\tSetting up basic default environment"
        puts stderr "\n\tVersion $dotversion\n"
}

module-whatis   "sets up basic default environment"

# for Tcl script use only
set     dotversion      3.2.10

###Make file deletes and moves interactive to avoid overwriting.
set-alias       "rm"    "rm -i"
set-alias       "cp"    "cp -i"
set-alias       "mv"    "mv -i"

prepend-path    PATH            /home/agram/python/anaconda3/bin
prepend-path    PATH            /home/agram/python/miniconda2/bin
```

Note that you will have to either source .bashrc or start a new terminal to activate the recent changes you made.

