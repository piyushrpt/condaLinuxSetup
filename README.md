# oldLinuxSetup

Setup python environment using anaconda on old linux machines

We will only retain gcc from the old linux setup. Assuming gcc >= 4.3.

### Miniconda2

- Download miniconda 2.7 64-bit bash installer from conda.pydata.org/miniconda.html
- chmod +x Miniconda2-latest-Linux-x86_64.sh
- Execute Miniconda2-latest-Linux-x86_64.sh 
   - Pick an install location, e.g, /home/piyush/python/miniconda2
- Add "bin" directory to PATH (pre-pend it).
- conda update --all 
   - To make sure that python2 is up to date
- conda install scons


### Anaconda3

- Download anaconda 3.5 64-bit bash installer from 
