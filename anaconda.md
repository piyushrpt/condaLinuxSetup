# Setting up Python2 and Python3 using Anaconda

If you already have python2.7 installed, install "scons" for it.
If you already have "scons" as well, you can skip the miniconda2 instructions.

**Latest Update: scons is now available for python3. You can use anaconda3 to install scons. Installing miniconda2 is completely optional.**

### Miniconda2

- We only need python2 for scons. You can instead choose to install all of anaconda2 as well, if needed.
- Download miniconda 2.7 64-bit bash installer from conda.pydata.org/miniconda.html
- Change permissions on the downloaded script
```bash
chmod +x Miniconda2-latest-Linux-x86_64.sh
```

- Execute Miniconda2-latest-Linux-x86_64.sh 
   - Pick an install location, e.g, /home/agram/python/miniconda2
```bash
./Miniconda2-latest-Linux-x86_64.sh 
```
   
- Update the miniconda installation
```bash
/home/agram/python/miniconda2/bin/conda update --all 
```
- Install scons
```bash
/home/agram/python/miniconda2/bin/conda install scons
```


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

- Link conda from anaconda3 to conda3
```bash
ln -s /home/agram/python/anaconda3/bin/conda /home/agram/python/anaconda3/bin/conda3
```

- You only need this command if you really want to work with the latest version of some library. I primarily used it to get the latest gdal library (before version 2.0 became available by default). This can break your regular setup if you are not familiar with using conda. So use this command with caution. Add conda-forge to list of channels for the latest libraries
```bash
/home/agram/python/anaconda3/bin/conda3 config --append channels conda-forge
```


- Update the anaconda installation
```bash
/home/agram/python/anaconda3/bin/conda3 update --all
```

- Create a link for cython3 
```bash
ln -s /home/agram/python/anaconda3/bin/cython /home/agram/python/anaconda3/bin/cython3
```
This is the only mechanism for distinguishing between cython2 and cython3 (same as python2 and python3).


### Anaconda3 package management

```bash
/home/agram/python/anaconda3/bin/conda3 remove --features mkl   (If conda uses mkl. This will get rid of annoying messages from mkl)
/home/agram/python/anaconda3/bin/conda3 install krb5
/home/agram/python/anaconda3/bin/conda3 install gdal
export GDAL_DATA="/home/agram/python/anaconda3/share"
/home/agram/python/anaconda3/bin/conda3 install libgdal
/home/agram/python/anaconda3/bin/conda3 install -c omnia fftw3f=3.3.4
```
