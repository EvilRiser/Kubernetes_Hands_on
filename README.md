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
```
$ cat <<EOF | sudo tee /etc/sysctl.d/99-kubernetes-cri.conf
> net.bridge.bridge-nf-call-iptables = 1
> net.ipv4.ip_forward = 1
> net.bridge.bridge-nf-call-ip6tables = 1
> EOF
```
create a file in **/etc/sysvtl.d** called **kubernetes-cri.conf**  
These are configurations needed for kubernetes networking.  
We need to make sure these settings are set when server is set and we will apply it immediately
```
sudo sysctl --system
```

Next after this we can install containerd package
```
$ sudo apt-get update && sudo apt-get install -y containerd.io
```
Now my containerd package is installed, we can set containerd configuration file.
```
$ sudi mkdir -p /etc/containerd
```
This will contain my containerd configuration
```
sudo containerd config default | sudo tee /etc/containerd/config.toml 
```
containerd config default will generate configurations and that I'm storing in .toml file
  
Just to make sure my conatinerd is using my configuration file I will restart containerd and check status if it is up and running
```
$ sudo systemctl restart containerd
$ sudo systemctl status containerd
```

Now we can install kubernetes packages, and it is necessary to disable swap

```
$ sudo swapoff -a
```

And install some packages which will be needed during installation process.

```
$ sudo apt-get update && sudo apt-get install -y apt-transport-https curl
```

Download and add GPG key:
```
$ curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
```
Then I can setup my repository configurations in **_/etc/apt/sources.list.d_**

[Cluster]: ./img/LAB01_Building_a_Kubernetes_1.20_Cluster_with_Kubeadm.png "Kubernetes Cluster"
