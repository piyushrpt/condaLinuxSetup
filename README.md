# oldLinuxSetup

Setup python environment using anaconda on old linux machines

We will only retain gcc from the old linux setup. Assuming gcc >= 4.3.

### Miniconda2

- We only need python2 for scons.
- Download miniconda 2.7 64-bit bash installer from conda.pydata.org/miniconda.html
- chmod +x Miniconda2-latest-Linux-x86_64.sh
- Execute Miniconda2-latest-Linux-x86_64.sh 
   - Pick an install location, e.g, /home/agram/python/miniconda2
- Add "bin" directory to PATH (pre-pend it).
- conda update --all 
   - To make sure that python2 is up to date
- conda install scons


### Anaconda3

- Download anaconda 3.5 64-bit bash installer from www.continuum.io/downloads
- chmod +x Anaconda3-4.0.0-Linux-x86_64.sh
- Execute Anaconda3-4.0.0-Linux-x86_64.sh
   - Pick an install location, e.g, /home/agram/python/anaconda3
- Add "bin" directory to PATH (make sure miniconda2 comes first and then anaconda3)
- Link conda from anaconda3 to conda3
    - ln -s /home/agram/python/anaconda3/bin/conda /home/agram/python/anaconda3/bin/conda3
- conda3 update --all
- Add "lib" directory of anaconda3 install to LD_LIBRARY_PATH

### Anaconda3 package management

- conda3 remove --features mkl   (If it gets built with mkl. This will get rid of annoying messages from mkl)
- conda3 install krb5
- conda3 install gdal
 - set env variable GDAL_DATA to /home/agram/python/anaconda3/share
- conda3 install libgdal
- conda3 install netcdf4
- conda install -c omnia fftw3f=3.3.4
  
