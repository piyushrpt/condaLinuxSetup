# Common issues

This page is a collection of common issues that one may encounter when trying to setup ISCE with anaconda.
Anaconda is a powerful package manager but some incompatibilities within different libraries does creep in.
This section attempts to address some of these issues:

##libjpeg.so.8 missing / incompatible
Soln:
The most common cause for this is that your GDAL version is not in sync with other libraries. 

```bash
conda3 update gdal
```

##  "from osgeo import gdal" doesn't work
Soln:
The most common case for this is that you dont have libgdal installed on your machine or it is outdated.

```bash
conda3 install libgdal

(or)

conda3 update libgdal
```

## libuuid.so missing / incompatible
Soln:
Some versions of gdal have libuuid listed as a dependency by error. You can fix this by uninstalling libuuid

```bash
conda3 uninstall libuuid
```

## Recode from CP437 to UTF-8 failed with the error: "Invalid argument"
Soln:
Set the environment variable CPL_ZIP_ENCODING=UTF-8
```bash
export CPL_ZIP_ENCODING=UTF-8
```
