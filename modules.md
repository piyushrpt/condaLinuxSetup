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
module load use.own
```
