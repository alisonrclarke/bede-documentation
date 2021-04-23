Environments
------------

The default software environment on Bede is called “builder”. This uses
the modules system normally used on HPC systems, but provides a system
of intelligent modules. To see a list of what is available, executing
the command ``module avail``.

In this scheme, modules providing access to compilers and libraries
examine other modules that are also loaded and make the most appropriate
copy (or “flavour”) of the software available. This minimises the
problem of knowing what modules to choose whilst providing access to all
the combinations of how a library can be built.

For example, the following command gives you access to a copy of FFTW
3.3.8 that has been built against GCC 8.4.0:

::

   $ module load gcc/8.4.0 fftw/3.3.8
   $ which fftw-wisdom
   /opt/software/builder/developers/libraries/fftw/3.3.8/1/gcc-8.4.0/bin/fftw-wisdom

If you then load an MPI library, your environment will be automatically
updated to point at a copy of FFTW 3.3.8 that has been built against GCC
8.4.0 and OpenMPI 4.0.5:

::

   $ module load openmpi/4.0.5
   $ which fftw-wisdom
   /opt/software/builder/developers/libraries/fftw/3.3.8/1/gcc-8.4.0-openmpi-4.0.5/bin/fftw-wisdom

Similarly, if you then load CUDA, the MPI library will be replaced by
one built against it:

::

   $ which mpirun
   /opt/software/builder/developers/libraries/openmpi/4.0.5/1/gcc-8.4.0/bin/mpirun
   $ module load cuda/10.2.89
   $ which mpirun
   /opt/software/builder/developers/libraries/openmpi/4.0.5/1/gcc-8.4.0-cuda-10.2.89/bin/mpirun

Modules follow certain conventions:

-  Logs of software builds can be found under ``/opt/software/builder/logs/``.
-  Installation recipes for modules can be found under directory ``/home/builder/builder/``.
-  Although modules do their best to configure your environment so
   that you can use the software, it is sometimes useful to know where the
   software is installed on disk. This is provided by the ``<NAME>_HOME``
   environment variable, e.g. if the ``gcc/8.4.0`` module is loaded,
   environment variable ``GCC_HOME`` points to the directory containing
   its files.
-  Software provided by modules sometimes use other modules for their
   functionality. It is not normally required to explicitly load
   these prerequisites but it can be useful, for example to mirror R's
   buld environment when installing an R library. Where this occurs,
   a list of modules is provided by the ``<NAME>_BUILD_MODULES``
   environment variable, e.g. the ``r`` module sets environment variable
   ``R_BUILD_MODULES``.

Software can be built on top of these modules in the following ways:

-  Traditional - loading appropriate modules, manually unpacking,
   configuring, building and installing the new software
   (e.g. ``./configure; make; make install``)
-  `Spack <https://spack.readthedocs.io/>`__ - automated method of
   installing software. Spack will automatically find the multiple
   flavours (or variants, in spack-speak) of libraries provided by
   builder, minimising the number of packages needing to be built.

With Builder and Spack, the opportunity arises for a project to inherit
and supplement software, and for users to then inherit and supplement
that in turn. In this way, the centre can concentrate on providing core
software of general use and allow projects and users to concentrate on
specialist software elements that support their work.

In addition, there are two other types of software environment on Bede,
which are not currently recommended:

-  The vendor-supplied set of modules that originally came with the
   machine. To use these, execute:
   ``echo ocf > ~/.application_environment`` and then login again.
-  Easybuild - an automated method of installing software, rival to
   Spack. To use this, execute:
   ``echo builder > ~/.application_environment`` and then login again.

In both cases, executing ``rm ~/.application_environment`` and login
again will return you to the default software environment.

Spack
~~~~~

Spack can be used to extend the installed software on the system,
without requiring specialist knowledge on how to build particular pieces
of software. Documentation for the project is here:
https://spack.readthedocs.io/

To install spack, execute the following and then login again:

::

   $ git clone https://github.com/spack/spack.git $HOME/spack

   $ echo 'export SPACK_ROOT=$HOME/spack' >> ~/.bash_profile
   $ echo 'source $SPACK_ROOT/share/spack/setup-env.sh' >> ~/.bash_profile

Example usage, installing an MPI aware, GPU version of gromacs and than
loading it into your environment to use (once built, execute
``spack load gromacs`` before using):

::

   $ spack install gromacs +mpi +cuda

Other useful spack commands: \* ``spack find`` - show what packages have
been installed \* ``spack list`` - show what packages spack knows how to
build \* ``spack compilers`` - show what compilers spack can use \*
``spack info <package>`` - details about a package, and the different
ways it can be built \* ``spack spec <package>`` - what pieces of
software a package depends on

If a project wishes to create a spack installation, for example under
``/projects/<project>/spack`` and you would like an easy way for your
users to add it to their environment, please contact us and we can make
a module.

If you are a user who wishes to supplement your project’s spack
installation, follow the installation instructions above and then tell
it where your project’s copy of spack is:

::

   cat > $SPACK_ROOT/etc/spack/upstreams.yaml <<EOF
   upstreams:
     spack-central:
       install_tree: /projects/<project>/spack
       modules:
         tcl: /projects/<project>/spack/share/spack/modules
   EOF

Easybuild
~~~~~~~~~

Not currently recommended.

The central Easybuild modules are available when a user executes the
following command and then logs in again:

::

   echo easybuild > ~/.application_environment

A user can create their own Easybuild installation to supplement (or
override) the packages provided by the central install by:

::

   echo 'export EASYBUILD_INSTALLPATH=$HOME/eb' >> ~/.bash_profile
   echo 'export EASYBUILD_BUILDPATH=/tmp' >> ~/.bash_profile
   echo 'export EASYBUILD_MODULES_TOOL=Lmod' >> ~/.bash_profile
   echo 'export EASYBUILD_PARALLEL=8' >> ~/.bash_profile
   echo 'export MODULEPATH=$HOME/eb/modules/all:$MODULEPATH' >> ~/.bash_profile

Login again, and then:

::

   wget https://raw.githubusercontent.com/easybuilders/easybuild-framework/develop/easybuild/scripts/bootstrap_eb.py
   python bootstrap_eb.py $EASYBUILD_INSTALLPATH

Verify install by checking sensible output from:

::

   module avail   # should show an EasyBuild module under user's home directory
   module load EasyBuild
   which eb       # should show a path under the user's home directory

Software can now be installed into the new Easybuild area using
``eb <package>``

Project Easybuild installations can be created using a similar method.
In this case, a central module to add the project’s modules to a user’s
environment is helpful, and can be done on request.
