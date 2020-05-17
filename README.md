# Using conda for developing/ using ISCE


## Using isce2 conda package as an end user

This section is for users who are not interested in modifying underlying C/C++ code, but just want to install isce and use the available workflows and contributed scripts. End users will be available to install binaries for release versions directly without having to set up compilers etc. 

For details, see [here](./user/condauser.md)

## Setting up a developer environment 

This section is for users who are interested in building ISCE from scratch, and want to make changes to underlying code. With a developer environment, users can also work directly with the latest version of the source code on repository and not rely on releases (typically done twice a year).

The setup of ISCE on your machine is broken down into 3 parts:

1. [Anaconda setup for python3](./dev/anaconda.md)
2. [Modules setup for managing environment](./dev/modules.md)
3. [Setting up ISCE](./dev/isceSetup.md)

## Known issues
4. [Common error messages / issues](./issues.md)

Alternate methods for setting up ISCE using anaconda exist. One can be found here: [https://github.com/scottyhq/isce_notes](https://github.com/scottyhq/isce_notes)
