Common tools and libraries
==========================

BLAS/LAPACK
-----------

The following numerical libraries provide optimised CPU implementations of BLAS and LAPACK on the system:

- ESSL (IBM Engineering and Scientific Subroutine Library)
- OpenBLAS

The modules for each of these libraries provide some convenience environment variables: ``N8CIR_LINALG_CFLAGS`` contains the compiler arguments to link BLAS and LAPACK to C code; ``N8CIR_LINALG_FFLAGS`` contains the same to link to Fortran. When used with variables such as ``CC``, commands to build software can become entirely independent of what compilers and numerical libraries you have loaded, e.g.

::

   module load gcc essl/6.2
   $CC -o myprog myprog.c $N8CIR_LINALG_CFLAGS


MPI
---

The main supported MPI on the system is OpenMPI.

For access to a cuda-enabled MPI: ``module load gcc cuda openmpi``

We commit to the following convention for all MPIs we provide as modules:

- The wrapper to compile C programs is called ``mpicc``
- The wrapper to compile C++ programs is called ``mpicxx``
- The wrapper to compile Fortran programs is called ``mpif90``


HDF5
----

When loaded in conjunction with an MPI module such as ``openmpi``, the
``hdf5`` module provides both the serial and parallel versions of the
library. The parallel functionality relies on a technology called MPI-IO,
which is currently subject to the following known issue on Bede:

- HDF5 does not pass all of its parallel tests with OpenMPI 4.x. If
  you are using this MPI and your application continues to run but does
  not return from a call to the HDF5 library, you may have hit a similar
  issue. The current workaround is to instruct OpenMPI to use an alternative
  MPI-IO implementation with the command: ``export OMPI_MCA_io=ompio``
  The trade off is that, in some areas, this alternative is extremely slow
  and so should be used with caution.


NetCDF
------

The ``netcdf`` module provides the C, C++ and Fortran bindings for this
file format library. When an MPI module is loaded, parallel support is
enabled through the PnetCDF and HDF5 libraries.

Use of NetCDF's parallel functionality can use HDF5, and so is subject
to its known issues on Bede (see above).


Cryo-EM Software Environment
----------------------------

Documentation on the the Cryo-EM Software Environment for Life Sciences is available :download:`here <Cryo-EM_Bede.pdf>`. Note that this document is mainly based on the installation on `Satori <https://mit-satori.github.io>`_ and might have some inconsistencies with the Bede installation.
