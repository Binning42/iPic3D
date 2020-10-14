How-to install and run iPic3D on your (local) Linux machine.
Doesn't need sudo or root permissions, everything could be installed locally.

1. Install HDF5 library. The current version working properly with iPIC3D is 1.8.9
http://www.hdfgroup.org/ftp/HDF5/releases/

$ cd <top HDF5 source code directory>
$ ./configure --prefix=<location for HDF5 software> --enable-parallel
$ make 
$ make check
$ make install 


2. Install the MPICH (or OpenMPI) that supports -lmpe library. For example, this one:
https://www.mpich.org/downloads/
During the installation of MPICH follow all the instructions in the README file.

Newer versions of OpenMPI should be fine by default in a linux distro.


3. Set environment variables:

$ vi ~/.bashrc
Add these at end of file:

    PATH=mpich_install_path/bin/:$PATH
    PATH=hdf5_install_path/lib/:$PATH
    PATH=hdf5_install_path/include/:$PATH
    LD_LIBRARY_PATH=hdf5_install_path/lib/:$LD_LIBRARY_PATH

Note: these paths can also go at the beginning of the makefile.

where mpich_install_path is the path to the installed MPICH2, and hdf5_install_path is the path to the installed HDF5.

4. Install H5hut. Recommanded version is 1.99.12
https://gitlab.psi.ch/H5hut/src/-/tags

$ cd <top H5hut source code directory>
$ ./configure --prefix=<location for H5hut software> --enable-parallel --with-hdf5=<location for HDF5 software>
$ make
$ make test (optional)
$ make install
$ vi ~/.bashrc
Add these at end of file:

    PATH=H5hut_install_path/include/:$PATH
    PATH=H5hut_install_path/lib/:$PATH


5. Check out the repository:

$ svn co https://svn.esat.kuleuven.be/iPIC/iPic3D/trunk/ iPic3D_folder

where iPic3D_folder is the desired folder for the code.

6. Compiling the code

$ cd iPic3D_folder

$ cp makefiles/makefile.YourSystem makefile

where the makefile.YourSystem is a makefile suitable for your system.
Set the following paths in the makefile:

INC_HDF5 = -I/hdf5_install_path/include
LIB_HDF5 = -L/hdf5_install_path/lib
HDF5LIBS = -L/hdf5_install_path/lib/ -lhdf5 -L/hdf5_install_path/lib/ -lhdf5_hl 
MPELIB = -L/mpich2_install_path/lib/ -lmpe

Finally, make everything:
$ make

7. Running iPic3D

$ mpiexec (or mpirun) -n 4 ./iPic3D inputfiles/GEM.inp
