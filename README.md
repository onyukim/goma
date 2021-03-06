# Goma 6.1
![](http://129.24.4.70:8080/buildStatus/icon?job=GomaMaster "Build Status")

A Full-Newton Finite Element Program for Free and Moving Boundary Problems with Coupled Fluid/Solid Momentum, Energy, Mass, and Chemical Species Transport

For more information see the [Goma website](http://goma.github.io)

Changes in version 6.1

* Automatic creation of brk files (see `docs/parallel_integration.md`)
* Bug fixes (mostly MPI related bugs)
* Epetra is now a supported matrix format (see `docs/epetra_matrix_format.md`)
* MUMPS is now supported through Amesos
* An experimental build script to build library dependencies is available under `scripts/`



## Build Instructions

### Get the Goma source code

Run the command

`git clone https://github.com/goma/goma`

### Get the prerequisites

Goma requires many third party libraries (TPLs, listed in the [scripts/READE.md](https://github.com/goma/goma/blob/master/scripts/README.md)) which can be built automatically with the build script located in the scripts directory.

#### Build script requirements

The build-goma-dep-trilinos-12.sh script relies on several packages readily available in many repositories.

For Ubuntu this will install the necessary packages to run the script:

`sudo apt-get install build-essential m4 zlib1g-dev libx11-dev gfortran`

For CentOS

`sudo yum install gcc-c++ gcc-gfortran m4 zlib-static libX11-devel`

The [scripts/README.md](https://github.com/goma/goma/blob/master/scripts/README.md) gives a more specific list of minimum requirements. Please ensure the script requirements are met before attempting to use it.

#### Build the TPLs automatically

Use these commands:

`cd goma`

`gomadir=$(pwd)`

Set a temporary variable that helps the user complete the install at their site.

`mkdir TPLs`

`cd scripts`

`nice ./build-goma-dep-trilinos-12.sh -j2 ${gomadir}/TPLs`

This will take a long time (hours). `nice` lowers the process priority, effectively backgrounding the build script and allowing continued use of the computer. The `-j2` flag indicates that two processes will be used to build the TPLs.

`cd ..`

Test that these libraries built successfully by checking for aprepro.

`${gomadir}/TPLs/trilinos-12.6.3/bin/aprepro -v`

If the libraries built successfully, this will print the version of the SEACAS Algebraic Preprocessor, if not address any errors before moving on.

### Configure and build Goma

Now follow these commands/instructions to build and install Goma:

* `mkdir build`
* `cd build`
* `cp ../cmake-config-example ../cmake-config`
* Copy the output of `echo ${gomadir}/TPLs` to the clipboard. Edit `../cmake-config`, find `GOMA_LIBS=` and paste the copied path after the `=` sign. This sets the value of the GOMA_LIBS variable to the path the TPLs were installed to.
* `../cmake-config`
* `make -j 2` To make with two processes, modify as necessary.
* `make install` 


### Run Goma

Many uses of Goma require the helper program `aprepro` to be on the path, ALSO the OpenMPI libraries must be on the library path. Make these environment changes OR use the handy wrapper script `${gomadir}/bin/rungoma`, generated by cmake. 

`cd ../..`

`goma/bin/rungoma`


### Run the tutorial

To get started with Goma, use the following:

* [Tutorial instructions](http://goma.github.io/files/goma-beginners-tutorial.pdf)
* [Tutorial files tarball](http://goma.github.io/files/goma_beginners_tutorial.tar.gz)
