Compilers
---------

All compiler modules set the ``CC``, ``CXX``, ``FC``, ``F90`` environment variables to appropriate values. These are commonly used by tools such as cmake and autoconf, so that by loading a compiler module its compilers are used by default.

This can also be done in your own build scripts and make files. e.g.

::

  module load gcc
  $CC -o myprog myprog.c

GCC
~~~

Note that the default GCC provided by Red Hat Enterprise Linux 7 (4.8.5)
is quite old, will not optimise for the POWER9 processor (either use
POWER8 tuning options or use a later compiler), and does not have
CUDA/GPU offload support compiled in. The module ``gcc/native`` has been
provided to point to this copy of GCC.

The copies of GCC available as modules have been compiled with CUDA
offload support:

::

   module load gcc/10.2.0

LLVM
~~~~

LLVM has been provided for use on the system by the ``llvm`` module.
It has been built with CUDA GPU offloading support, allowing OpenMP
regions to run on a GPU using the ``target`` directive.

Note that, as from LLVM 11.0.0, it provides a Fortran compiler called
``flang``. Although this has been compiled and can be used for
experimentation, it is still immature and ultimately relies on
``gfortran`` for its code generation. The ``lvm/11.0.0`` module therefore
defaults to using the operating system provided ``gfortran``, instead.
