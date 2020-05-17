# Common issues

This page is a collection of common issues that one may encounter when trying to setup ISCE with anaconda.
Anaconda is a powerful package manager but some incompatibilities within different libraries does creep in.
This section attempts to address some of these issues. We will try to time tag the issues - not all of them maybe relevant to your use case.


## scons cannot find packages

### clear local cache
When users update the SConfigISCE file and try to rerun scons, it does not pick up the new configuration. It looks for cached results in the local folder first.
So clean up files like .sconf\_temp and .sconsign.dblite before attempting to run scons install again.

### debugging errors
scons generates a file named config.log with the exact tests that were used by it to determine availability of include files / libraries. This file should contained detailed error messages on why it failed to find a required component. You can rerun these compile/link lines in your terminal to debug the issue. 

## conda: undefined reference to uuid ..  (2018-07)

There seems to be an issue with conda's handling of libuuid versions. This causes failure to recognize that libXm and libXt are available on the machine.

Solution:

1. Find path to libuuid used by libXm and libSM
```bash
> ldd /lib64/libSM.so | grep uuid
> ls -ltr /lib64/libuuid.so.1
```

2. Link libuuid from anaconda to this library
```bash
> cd /home/agram/anaconda3/lib
> unlink libuuid.so     (should be a link to libuuid.so.1.0.0)
> unlink libuuid.so.1   (should be a link to libuuid.so.1.0.0)
> ln -s /lib64/libuuid.so.1.3.0 libuuid.so
> ln -s /lib64/libuuid.so.1.3.0 libuuid.so.1
```


## conda: Recode from CP437 to UTF-8 failed with the error: "Invalid argument"
Soln:
Set the environment variable CPL_ZIP_ENCODING=UTF-8
```bash
export CPL_ZIP_ENCODING=UTF-8
```
