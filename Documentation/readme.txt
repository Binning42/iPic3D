How-to install and run iPic3D on your (local) Linux machine.
Doesn't need sudo or root permissions, everything could be installed locally.

1. Install HDF5 library. The current version working properly with iPIC3D is 1.8.9 <br/>
http://www.hdfgroup.org/ftp/HDF5/releases/

```
$ cd <top HDF5 source code directory>
$ ./configure --prefix=<location for HDF5 software> --enable-parallel
$ make 
$ make check
$ make install
```

2. Install the MPICH (or OpenMPI) that supports -lmpe library. For example, this one:
https://www.mpich.org/downloads/ <br/>
During the installation of MPICH follow all the instructions in the README file. <br/>
Newer versions of OpenMPI should be fine by default in a linux distro.

3. Set environment variables:

```
$ vi ~/.bashrc
Add these at end of file:

    PATH=mpich_install_path/bin/:$PATH
    PATH=hdf5_install_path/lib/:$PATH
    PATH=hdf5_install_path/include/:$PATH
    LD_LIBRARY_PATH=hdf5_install_path/lib/:$LD_LIBRARY_PATH
```
Note: these paths can also go at the beginning of the makefile. <br/>

where mpich_install_path is the path to the installed MPICH2, and hdf5_install_path is the path to the installed HDF5.

4. Install H5hut. Recommanded version is 1.99.12 <br/>
    https://gitlab.psi.ch/H5hut/src/-/tags

    Requirements:
- autotools (libtool, automake, autoconf >= 2.60)
- C compiler supporting C99 (for example GCC >= 4.5)
- MPI for parallel version (for example openmpi)
- HDF5 >= 1.8.x
- (optional) VTK to convert triangle or tetrahedral meshes from legacy VTK format to H5hut’s mesh format
```
$ sudo yum install libtool (optional)
$ cd <top H5hut source code directory>
$ ./autogen.sh
$ ./configure --prefix=<location for H5hut software> --enable-parallel --with-hdf5=<location for HDF5 software>
$ make
$ make test (optional)
$ make install
$ vi ~/.bashrc
Add these at end of file:

    PATH=H5hut_install_path/include/:$PATH
    PATH=H5hut_install_path/lib/:$PATH
```
5. Compiling the code
```
$ cd iPic3D_folder
$ cp makefiles/makefile.YourSystem makefile
where the makefile.YourSystem is a makefile suitable for your system.

Set the following paths in the makefile:

    HDF5_HOME   = /usr/local/hdf5_install_path
    H5HUT_HOME  = /usr/local/H5hut_install_path

$ make
```
6. Create the folder to store data
```
$ mkdir path_save_data (default is named data)
$ cd inputfiles
$ vi GEM.inp
    SaveDirName    = /path_save_data (default is named data)    # Output directory
    RestartDirName = /path_save_data (default is named data)    # Restart directory (for WriteMethod=default)

```

7. Running iPic3D
```
$ mpiexec (or mpirun) -n 4 ./iPic3D inputfiles/GEM.inp
```