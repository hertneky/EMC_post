.. role:: underline
    :class: underline
.. role:: bolditalic
    :class: bolditalic

*********************
Software Installation
*********************

=====================
Software Requirements
=====================

Before installing the UPP code, it is necessary to ensure that you have the required libraries available on
your system. These libraries include:

  - Unidata's NetCDF library
    https://www.unidata.ucar.edu/software/netcdf/

  - The NCEP libraries for the UPP application
    https://github.com/NCAR/NCEPlibs
      - For instructions on building the NCEP libraries required for UPP, please refer to the README
        document in the NCEPlibs directory.

      .. note::
         These are specific versions of the NCEP libraries maintained by NCAR/DTC and other versions
         of NCEPlibs may not work.

The UPP has some sample visualization scripts included to create graphics using:

  - GrADS (http://cola.gmu.edu/grads/gadoc/gadoc.php)

  - GEMPAK (http://www.unidata.ucar.edu/software/gempak/index.html)

  .. note::
     These are not part of the UPP installation and need to be installed separately if one would like to use
     either plotting package. For more information on these, please reference the "Visualization" section.

UPP has been tested on LINUX platforms (with PGI, Intel and GFORTRAN compilers).

======================
Obtaining the UPP Code
======================

The UPP package is available on Github. To create a local copy of the remote UPP repository on
your computer including the required CRTM submodules:

      | :bolditalic:`git clone -b "release-tag-name" --recurse-submodules https://github.com/NOAA-EMC/EMC_post
        UPPV4.1`

      | where, :bolditalic:`release-tag-name` is the release tag you wish to clone (e.g. for **UPPV4.1** use the release tag
        :bolditalic:`dtc_post_v4.1.0`).

This will clone the specified release of the EMC_post repository into a directory called **UPPV4.1**.

.. note::
   Always obtain the latest version of the code if you are not trying to continue a pre-existing project.

   This documentation assumes the top directory of the UPP is called **UPPV4.1**.
   
=======================
UPP Directory Structure
=======================

Under the main directory of **UPPV4.1** reside the following relevant subdirectories (* indicates directories
that are created after the configuration step):

     | **exec***: Contains the :bolditalic:`unipost` executable after successful compilation

     | **include***: Source include modules built/used during compilation of UPP

     | **lib***: Libraries built/used by UPP that are separate from NCEPlibs

     | **parm**: Contains parameter files, which can be modified by the user to control how the post processing is performed.

     | **scripts**: Contains sample run scripts to process fv3 history files.
     |   - **run_unipost**: run :bolditalic:`unipost`.
     |   - **run_unipostandgempak**: run :bolditalic:`unipost` and GEMPAK to plot various fields.
     |   - **run_unipostandgrads**: run :bolditalic:`unipost` and GrADS to plot various fields.

     | **sorc**: Contains source codes for:
     |   - **arch**: Machine dependent configuration build scripts used to construct :bolditalic:`configure.upp`
     |   - **comlibs**: Contains source code subdirectories for the UPP libraries not included in NCEPlibs
     |       - **crtm2**: Community Radiative Transfer Model library
     |       - **wrfmpi_stubs**: Contains some C and FORTRAN codes to generate :bolditalic:`libmpi.a` library used to replace MPI
                                 calls for serial compilation
     |       - **xml**: XML support for the GRIB2 parameter file
     |   - **ncep_post.fd**: Source code for :bolditalic:`unipost`

=======================
Installing the UPP Code
=======================

Before installing UPP, the following environment variables must be set:

     | :bolditalic:`setenv NETCDF /path/to/netcdf`
     | :bolditalic:`setenv NCEPLIBS_DIR /path/to/NCEPlibs`

To reference the netCDF libraries, the configure script checks for an environment variable (:bolditalic:`NETCDF`) first,
then the system default (**/user/local/netcdf**), and then a user supplied link (:bolditalic:`./netcdf_links`). If none
of these resolve a path, the user will be prompted by the configure script to supply a path.

To reference the NCEP libraries, the configure script checks for an environment variable (:bolditalic:`NCEPLIBS_DIR`).

Type configure, and provide the required info. For example:

     | :bolditalic:`./configure`

You will be given a list of choices for your computer.

.. note::
   At this time, the option to build serially does not work

:underline:`Choices for LINUX operating systems are as follows:`

1. Linux x86_64, PGI compiler (serial)
2. Linux x86_64, PGI compiler (dmpar)
3. Linux x86_64, Intel compiler (serial)
4. Linux x86_64, Intel compiler (dmpar)
5. Linux x86_64, Intel compiler, SGI MPT (serial)
6. Linux x86_64, Intel compiler, SGI MPT (dmpar)
7. Linux x86_64, gfortran compiler (serial)
8. Linux x86_64, gfortran compiler (dmpar)

Choose one of the :bolditalic:`dmpar` configure options listed. If the serial option is chosen during configuration, an
error staement will be printed. Check the :bolditalic:`configure.upp` file created and edit for compile options/paths, if
neccessary. For debug flag settings, the configure script can be run with a :bolditalic:`-d` switch or flag.

To compile UPP, enter the following command:

     | :bolditalic:`./compile >& compile_upp.log &`

When compiling with distributed memory (serial) this command should create 2 (3) UPP libraries in
**UPPV4.1/lib/** (:bolditalic:`libCRTM.a`, :bolditalic:`(libmpi.a)`, :bolditalic:`libxmlparse.a`) and the UPP executable in **UPPV4.1/exec/**
(:bolditalic:`unipost.exe`).

To remove all built files, as well as the :bolditalic:`configure.upp`, type:

     | :bolditalic:`./clean`

This action is recommended if a mistake is made during the installation process or a change is made to the
configuration or build environment. There is also a :bolditalic:`clean -a` option which will revert back to a pre-install
configuration.

.. note::
   For building **UPPV4.1** on operational NCEP machines (hera/jet/wcoss), just type :bolditalic:`./compile machine_name` (e.g. :bolditalic:`./compile hera`).   This is an option for **UPPV4.1** only and will not work for any previous release versions.
