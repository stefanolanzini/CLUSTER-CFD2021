# OPENMPI

OpenMPI is an open source software for Message Passing Interface implementation. Being a Message Passing
Interface (MPI), it is used in computer science for invoking behavior (ie. running a program) on a computer.
The invoking program sends a message to a process and relies on that process and its supporting infrastructure
to then select and run some appropriate code. For CFD simulations, it’s crucial to exploit OpenMPI to split
the simulation in different processes that can run in parallel. The maximum number of parallel processes is
equal to the total number of cores in the cluster.
OpenMPI 4.1.1 was compiled and built on each node starting from the source code. The source code can be
downloaded from the software web page https://www.open-mpi.org/. The command to extract the archive is:

**$ tar -xf openmpi-4.1.1.tar.bz2**

In the "README" file which can be found in the unzipped folder, the commands to compile and build the
code are:

**$ tar -xf openmpi-4.1.1.tar.bz2**

**$ make all install**

With this final command, the installation is complete.
