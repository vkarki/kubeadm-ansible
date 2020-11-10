# Ansible playbook to install Kubernetes Cluster using kubeadm
The following documentation can be followed to set up a Kubernetes cluster on __Ubuntu 18.04 LTS__.

This documentation guides in setting up a cluster with one master node and two worker node.

## Assumptions
|Role|FQDN|IP|OS|RAM|CPU|
|----|----|----|----|----|----|
|Masters|kmaster.example.com|172.16.16.100|Ubuntu 18.04|2G|2|
|Workers|kworker1.example.com|172.16.16.101|Ubuntu 18.04|1G|1|
|Workers|kworker2.example.com|172.16.16.102|Ubuntu 18.04|1G|1|
|Ansible Controller| |172.16.16.99|Ubuntu 18.04|1G|1|

## Update the ansible.cfg
Update the value of Inventory file to the location where the host file exists.

## k8s-commons.yml
This YAML file contains the tasks which are applied across the master and worker nodes.
Below are the common tasks defined in the yml file.
1) Update /etc/hosts file
2) Disable firewall
3) Disable Swap 
4) Install required system packages
5) Install Docker and enable the docker service
6) Update the network bridge IP tables
7) Add K8s repository, install K8s components (kubeadm ,kubelet, kubectl) version 1.18.5-00 and enable kubelet on all the servers.

## k8s-master.yml
The tasks in the YAML file are applied to master server.
1) Start kubeadm.
2) Copy the admin.conf and store it in $HOME/.kube/config file 
3) Install calico as CNI by running kubectl command

## k8s-workers.yml
Below are the tasks in the YAML file.
1) Run the kudeadm join command and register the output.
2) Registered output is set as fact.
3) Access the fact from the master server and run the command in the worked nodes using shell module. 


