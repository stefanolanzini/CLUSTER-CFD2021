# CLUSTER-CFD2021
List of commands for the software installation

# SSH

SSH or Secure Shell is a network communication protocol that enables two or more computers to communicate,
access each other and share data. The server version of SSH is installed and started in all the nodes with the
following commands:

**$ sudo apt-get install ssh-server**

**$ sudo systemctl start ssh**

It’s possible to transfer files to a remote computer with:

**$ scp /pathtothefileinthelocalcomputer/file.txtcomputername@IPaddress:/pathtothelocationintheremoteserver**

To access the terminal of another node via the terminal of the host node, the following command can be used:

**$ ssh computername@IPaddress**

For example, to have access to a node with IP address 192.168.0.7, the following command must be used:

**$ ssh jetson@192.168.0.7**

To execute an SSH connection with another node, the remote computer’s password must be entered. This
is a problem for parallel computing were zillions of communication and data transfer takes place and would
require every time to insert the password. For this reason, it is necessary to generate automatic keys that
will substitute the password and will not require to enter the password to establish an SSH connection or to
communicate with other nodes. The keys can be created using the following command:

**$ sudo ssh-keygen -t -rsa**

Once the key of a node is created, it must be shared to other nodes

**$ ssh-copy-id pcname@IPaddress**

These two steps must be repeated for every node on the network.
To make SSH operations more streamlined and to avoid using IP-addresses of the nodes, it is possible to give
an "alias" name to the IP-addresses of the nodes. This is easily accomplished by editing the etc/host system
file in every node. At the bottom of this file it’s possible to add a look up table in which each node, which
has its unique IP-address, can be associated with an "alias" name . For example, the node having IP-address
192.168.0.9, can be renamed to nodo9 by adding the following line:

**192.168.0.9 nodo9**

This modified system file must be copied to every node. Now it’s possible to SSH connect between the nodes
simply by using the command:

**ssh nodo9**







