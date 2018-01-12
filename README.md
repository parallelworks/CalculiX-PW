CalculiX - Parallel Works
=========================

This repository contains the open source FEM software CalculiX with some modifications and some relevant tools.

current configuration
---------------------

-   Based on [CalculiX](http://www.calculix.de/) version 2.12 (multi-threaded version, see the notes below)

Installing CalculiX (multi-threads):
------------------------------------

We followed the directions for installing CalculiX Multi-Thread on Ubuntu from [How To Install CalculiX 2.6 Multi-Thread On Ubuntu](http://www.libremechanics.com/?q=node/9). For completeness the instructions for installing (on Ubuntu 16.04) are repeated below:

1.  Install the required tools for compiling multi-threaded CalculiX:
    -   gfortran
    -   make
    -   f2c
    -   liblapack3
    -   liblapack-dev
    -   libexodusii-dev
    -   libgl1-mesa-dev
    -   libglu1-mesa-dev
    -   libxi-dev
    -   libxmu-dev

    The above packages can be installed by

    ``` example
    apt-get install \
    gfortran \
    make \
    f2c   \
    liblapack3 \
    liblapack-dev  \
    libexodusii-dev   \
    libgl1-mesa-dev    \
    libglu1-mesa-dev \
    libxi-dev \
    libxmu-dev \
    ```

2.  If downloading the Spooles package from (<http://www.netlib.org/linalg/spooles/spooles.2.2.html>), make the following changes (**The changes have already been applied to the spooles files in this repository**):
    -   In `spooles.2.2/Tree/src/makeGlobalLib` change: `drawTree.c` to `draw.c`
    -   In `spooles.2.2/Make.inc` change: `CC = /usr/lang-4.0/bin/cc` to `CC = /usr/bin/cc`
3.  If downloaing the `ARPACK` (`arpack96.tar.Z`) and patch (`patch.tar.Z`) from <http://www.caam.rice.edu/software/ARPACK/> make the following changes (**The changes have already been applied to the spooles files in this repository**):
    -   In `ARPACK/ARmake.inc` change:
        -   `home = $(HOME)/ARPACK` to `home = /usr/local/ARPACK`
        -   `PLAT = SUN4` to `PLAT = linux`
        -   `FC = f77` to `FC = gfortran`
        -   `FFLAGS = -O -cg89` to `FFLAGS = -O2`
        -   `MAKE = /bin/make` to `MAKE = /usr/bin/make`
    -   In `ARPACK/UTIL/second.f` change: `EXTERNAL ETIME` to `*EXTERNAL ETIME`
4.  Compile spooles:
    1.  Change to the spooles directory:

        ``` example
        cd CalculiX-PW/spooles.2.2/    
        ```

    2.  Compile spooles

        ``` example
        make lib    && \
        cd MT/src/   && \
        make   
        ```

5.  Compile ARPACK:
    1.  **Make sure you update the `home` variable in `ccx-212-patch-PW/ARPACK/ARmake.inc`** **to the root of the `ARPACK` source tree (Top level of ARPACK directory)**
    2.  Change to ARPACK directory:

        ``` example
        cd CalculiX-PW/ARPACK/    
        ```

    3.  Compile ARPACK:

        ``` example
        make lib   
        ```

    4.  To remove the object files, but keep the library you could run

        ``` example
        make clean 
        ```

6.  Compile CalculiX: (The CalculiX makefile in this repository `CalculiX/ccx_2.6/src/Makefile` is already updated for multi-threading)

    ``` example
    cd CalculiX-PW/src  && \
    make        
    ```
