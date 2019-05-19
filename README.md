# rke Cluster with Vagrant and Ansible

## Setup
1. Generate an SSH key pair (`id_rsa` and `id_rsa.pub`) in the project root 
1. `vagrant up`. That's it.

## Cluster Architecture

Will setup 3 Nodes total: 

* 1 control plane node (running etcd as well; address: 192.168.100.100)
* 2 worker nodes (192.168.100.200 resp. .201)
* 1 extra node (exporting an NFS share; address: 192.168.100.250)
