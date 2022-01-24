# SU2

The open source software SU2 is used for the simulations. It has been installed in the fumo folder following
the instructions given in the GitHub page of the software LINK PAGINA SU2, FACCIAMO SITOGRAFIA.
To run simulations on multiple cores which are distributed on different nodes, a text file that contains all the
IP-addresses of the nodes and the total number of cores available on each node is needed. For this cluster, the
contents of this file named "hostfile" are :
192.168.0.7 slots=4
192.168.0.8 slots=4
8192.168.0.9 slots=4
192.168.0.10 slots=4
192.168.0.11 slots=4
The "hostfile" must be in the same folder where the configuration file of the simulation is present. To launch
a simulation with n cores, the following command could be used:

**/home/jetson/Code/OpenMPI/bin/mpirun -mca btltcpifinclude eth0 -np n –machinefile hostfile/mnt/fumo/fp/QuickStart/n
config.cfg > logn**

Note that the whole path to OpenMPI and SU2-CFD must be explicitly specified.
The option > logn at the end of the above command, writes the output of the simulation in a file named
logn.txt instead of printing it on the terminal. In each configuration file, the following line is added to retrevive
directly important performance indices.

** WRTPERFORMANCE= YES**

Due to large amount of simulations that had to be done for performance analysis, the whole procedure to launch
all the required simulations one after the another, was automated. An executable file named "ALLRUN" is
created which contains all the commands that need’s to be executed. Then from terminal, this excutable file
is launched using the following command :

** $ ./ALLRUN** 
