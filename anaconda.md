# Setting up Python2 and Python3 using Anaconda

**Latest Update: scons is now available for python3. You can use anaconda3 to install scons. One no longer needs python2 to build ISCE*

The following set of instructions work with:
Anaconda3-5.2.0-Linux-x86_64.sh downloaded on 2018-07-19.


### Anaconda3

- Download anaconda 3.5 64-bit bash installer from www.continuum.io/downloads
- Changer permissions on the downloaded script
```bash
chmod +x Anaconda3-4.0.0-Linux-x86_64.sh
```
- Execute Anaconda3-4.0.0-Linux-x86_64.sh
   - Pick an install location, e.g, /home/agram/python/anaconda3
```bash
./Anaconda-3.4.0.0-Linux-x86_64.sh
```

- Update the anaconda installation
```bash
/home/agram/python/anaconda3/bin/conda update --all
```

- Create a link for cython3 
```bash
ln -s /home/agram/python/anaconda3/bin/cython /home/agram/python/anaconda3/bin/cython3
```
This is the only mechanism for distinguishing between cython2 and cython3 (same as python2 and python3).


### Anaconda3 package management

Earlier versions of this set of instructions recommended adding conda-forge channel. This is no longer needed. Packages on the main channel reasonably new. If you have *conda-forge*** listed in your .condarc file - recommend removing it (unless you are an expert in handling inconsistencies that might crop up).

#### Get gcc5 (recommended)
```bash
/home/agram/python/anaconda3/bin/conda install -c gpugrid gcc-5
```

### Get scons
```bash
/home/agram/python/anaconda3/bin/conda install scons
```

### Get gdal
```bash
/home/agram/python/anaconda3/bin/conda3 install gdal
export GDAL_DATA="/home/agram/python/anaconda3/share"
```

### Get fftw3f
```bash
/home/agram/python/anaconda3/bin/conda3 install -c omnia fftw3f=3.3.4
```
