# Kubernetes_Hands_on

This is what we are trying to create the cluster looks like

![alt text][Cluster]


Three Servers: One control plane and two worker nodes

Plan:
____
1. Install Packages
2. Initialize the cluster
3. Install the Calico network add-on
4. Join the worker nodes to cluster


##### Install Packages
___
First ssh to your network  
In cmd write  
```
$ ssh "user_name@hostname" -y
```
Here -y to accept all conditions or use ssh --help to know more
```
$ password: 
```
give the password here and done connected to the server.  

<br>
So we need to install the packages in all three of our server so we just have to repeat these steps in all 3 server.
<br><br>
Start with installing and configuring containerd and in order to do that we need to enable some kernel modules:  

```
$ cat <<EOF | sudo tee /etc/modules-load.d/containerd.conf   
> overlay
> br_netfilter
> EOF
```
We create this to enable the **containerd.conf** file in **etc/modules-load.d**  <br> <br>
This file will instruct the server to install overlay and br_netfilter,
so everytime server starts up these kernel modules will be loaded.
And we also need to make sure that the kernels are loaded immediately and need not to restart the server.
```
$ sudo modprobe overlay
$ sudo modprobe br_netfilter
```

Next we need to set some system level configurations







[Cluster]: ./img/LAB01_Building_a_Kubernetes_1.20_Cluster_with_Kubeadm.png "Kubernetes Cluster"
