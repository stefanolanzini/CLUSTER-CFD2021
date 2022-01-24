# Distributed File System
In a cluster, each node is independent and has its own independent file system. To run a parallel program, in
our case a fluid dynamics simulation, the solver needs to have the same file system and the same input/output
files which are used by all the nodes at the same time. Usually, all the nodes are connected to each other via a
network that could use a LAN connection or a specialized network for HPC like Infiniband or Openfabric. To
this end, two different Distributed File System (DFS) has been installed in this cluster:
• NFSv3 Protocol
• Glusterfs
# NFSv3 Protocol
In the master node, the server version of NFSv3 protocol has been installed.

**$ sudo apt-get install nfs-server**

In all the rest of the slave nodes, the client version of NFSv3 protocol has been installed.

**$ sudo apt-get install nfs-client**

Once the NFS protocol has been installed in all the nodes, a new directory has to be created which will be
shared among all the nodes and will have the new distributed file system. The new directory has been created
in the root directory and is called nfs using the following command.

**$ sudo mkdir /nfs**

On the master node, this file system must be exported and to do so, a line has to be added to a specific file
which can be done using any editor. The file can be found in the /etc directory and is called exports. The line
to be added is :

**/nfs *(rw,sync)**

And then the nfs server has to be restarted which can be done using the following command

**$ sudo service nfs-kernel-server restart**

And on all the slave nodes, the /nfs directory must be mounted in order to use this file system. To do so, the
following line has to be added to the fstab file, which can be found in the /etc directory.

**nodo11:/nfs /nfs nfs**

And then finally to mount this directory, the following command must be run on all the slave nodes.
By default, the owner of this file system is the root user but usually the parallel programs like simulations are
executed using the normal user account. So, to change the ownership from root user to a normal user, in this
case, to Jetson, the following command can be used:

**$ sudo chown jetson /nfs**

With this final instruction, the NFSv3 file system is installed.

# GlusterFS
In GlusterFS, a basic storage unite is called a brick. A brick can be an external disk or a certain partition of a
disk. Each node can contain one or more bricks and the entire distributed file system is made up of these bricks
which are distributed among different nodes. By choosing which bricks make up a distributed file system, a
volume is created.
In GlusterFS, there are many types of volume architectures which have different usage. In this cluster, a
distributed GlusterFS volume has been created. The purpose of this architecture is to cheaply scale the volume
size by simply adding a brick, which could be an external storage device, to a volume. However, it doesn’t
provide redundancy and a failure of the volume will lead to a complete loss of the data stored in that volume.
To install GlusterFS, you need to add the GlusterFS PPA repository in all the nodes and in this cluster version
5 of GlusterFS has been installed. To add the repository, the following commands have been used.

**$ sudo apt-get install software-properties-common**

**$ sudo add-apt-repository ppa:gluster/glusterfs**

**$ sudo apt update**

On the master node, the server version of GusterFS has been installed using the command,

**$ sudo apt install glusterfs-server**

To enable it to run on system boot, the following command must be run,

**$ sudo systemctl enable glusterd**

On all the other slave nodes, the client version of GlusterFS has been installed.

**$ sudo apt install glusterfs-client**

Once GlusterFS has been installed, the next step is to create bricks on every node. In each node, a single brick
has been created which is named "gv1" in the folder /pool which is located in the root directory.

**$ sudo apt install glusterfs-client**

**$ sudo mkdir /pool**

**$ sudo mkdir /pool/gv1**

After creating a brick in every node, using the master node, a volume has been created using these five bricks
called "CFD2021" using the command,

**$ sudo gluster volume create CFD2021 transport tcp**

**$ nodo11:/pool/gv1 nodo10:/pool/gv1 nodo9:/pool/gv1 nodo8:/pool/gv1 nodo7:/pool/gv1**

Then the volume "CFD2021" can be started through the master node using the command,

**$ sudo gluster volume start CFD2021**

Once the volume has been created, it has to be mounted. The mounting point has been created in the following
root directory: /mnt/CFD2021. This step must be repeated in all the nodes.

**$ sudo mkdir /mnt/CFD2021**

To mount the volume, CFD2021 in the mounting folder /mnt/CFD2021, the following command should be
run in all the nodes.

**$ sudo mount -t glusterfs nodo11:/CFD2021 /mnt/CFD2021**

To automount the volume on system boot, the following line must be added to the system file /etc/fstab in
every node.

**nodo11:/CFD2021 /mnt/CFD2021 glusterfs defaults,_netdev 0 0**
