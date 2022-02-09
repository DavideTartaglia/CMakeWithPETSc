# CMakeWithPETSc
As a beginner, I struggeled a lot to link PETSc with CMake in a comfortable way. This is my best attempt to make it work decently.

This small repo contains the 2D Poisson problem exemple from the PETSc tutorial. I compile it using CMake and linking OpenMPI and PETSc as external libraries.

## Recommended setup
I tested this on Debian 10, but it should work on any linux environment.

## Prerequisites
* pkg
* CMake v3.11
* OpenMPI (PETSc offers the possibility to dowload it during its installation)
* PETSc (a version recent enough to have the `PETSc.pc` (or `petsc.pc`) file in `${PETSC_DIR}/${PETSC_ARCH}/lib/pkgconfig` )

I assumed you already installed successfully PETSc. I also assume that you have set the value for the environmental variable `${PETSC_DIR}` to be equal to the path to your favourite installation of PETSc. If you didn't, you can consider doing it by adding a line to your .bashrc sourcing a value for the `${PETSC_DIR}`. To do this, open with your favourite text editor the file .bashrc that you can find in you home directory and add this line at the bottom:

`export PETSC_DIR=path/to/your/petsc`

If you didn't, just remember that the value of `${PETSC_DIR}` is equal to the directory of your PETSc installation.

More or less the same holds for the value of the `${PETSC_ARCH}`. Check its recommended value during the installation of PETSc.

## How do I install this?
To me, the easiest way (and probably the recommended one, see https://github.com/jedbrown/cmake-modules#readme) to solve the problem of using PETSc with CMake is to use pkg-config.

The worst part is that you need to add a small line to your .bashrc file, that you can find in you home directory. To do this, open .bashrc in your home directory and add this line at the bottom:

`export PKG_CONFIG_PATH==/usr/lib/pkgconfig:${PETSC_DIR}/${PETSC_ARCH}/lib/pkgconfig`

so that pkg knows where to find your installation of petsc.

Afterwords, to install this project, after cloning it, just create a build folder and enter it

`mkdir build`

`cd build`

Then type:

`cmake ..`

`cmake --build .`

At this point everything should be set. To run the exemple, you can type:

`mpiexec -n 4 ./poisson2D -da_grid_x 3 -da_grid_y 3 -pc_type mg -da_refine 10 -ksp_monitor -ksp_view -log_view`

## Of course
this is just an exemple that offers a simple way to compile and link effectively a project that makes use of petsc and openmpi as third party libraries. To make an actually deliverable app you can consider different ways to do this. If you do find more elegant ways, don't hesitate to contact me :)
