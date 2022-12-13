<!DOCTYPE html>
<link rel="stylesheet" type="text/css" href="https://fonts.googleapis.com/css?family=Dosis&display=swap">
<html>
<body>

## Sidero Metal - Automation at the next level
![alt text](https://github.com/fontexD/k8s/blob/main/sidero/logo.jpg)

<i>What ive used in this Article</i>

| Application | Version |
| ------------- | ------------- |
| Kubernetes  | V1.24  |
| CentOS Stream  | V9.0  |
| Unifi DHCP| V7.0.2 |
| Libvirt/Kvm| V8.10.0 |

<br>
<b>This</b> is by far one of the best way to archive automation of K8s deployment and life cycle mangement ive come to experience so far, i used to do Terraform to provision VM 's and Ansible for configuration of the VM's, which is also and incredible nice way to do things!

<br>
<br>

But it does require some maintanence and understandig of Ansible , Terraform and the whole mangement part of this including you have to spend time keeping your OS for every node up to date, installing required packages, making sure there is no broken parts on your os, all this taking your time from atually using k8s and not just keeping it alive and up to date!

## So what is Sidero Metal and TalosOs? 

### Sidero Metal
Sidero (“Iron” in Greek) is a project created by the Sidero Labs team. Sidero Metal provides lightweight, composable tools that can be used to create bare-metal Talos Linux + Kubernetes clusters. These tools are built around the Cluster API project.

Because of the design of Cluster API, there is inherently a “chicken and egg” problem: you need an existing Kubernetes cluster in order to provision the management plane, that can then provision more clusters. The initial management plane cluster that runs the Sidero Metal provider does not need to be based on Talos Linux - although it is recommended for security and stability reasons. The Getting Started guide will walk you through installing Sidero Metal either on an existing cluster, or by quickly creating a docker based cluster used to bootstrap the process

#### Description from Sidero Metal Site https://www.sidero.dev/v0.5/overview/introduction/


### Talos os

Talos is a container optimized Linux distro; a reimagining of Linux for distributed systems such as Kubernetes. Designed to be as minimal as possible while still maintaining practicality. For these reasons, Talos has a number of features unique to it:

it is immutable
it is atomic
it is ephemeral
it is minimal
it is secure by default
it is managed via a single declarative configuration file and gRPC API
Talos can be deployed on container, cloud, virtualized, and bare metal platforms.

Why Talos
In having less, Talos offers more. Security. Efficiency. Resiliency. Consistency.

All of these areas are improved simply by having less.

#### Description from Talos Os Site https://www.sidero.dev/v0.5/overview/introduction/](https://www.talos.dev/v1.2/introduction/what-is-talos/)
<br>
<br>
    
        
## Lets get onto it!
---
| Server's | IP |
| ------------- | ------------- |
| Control-Plane  | 192.168.10.169  |
| LIbvirt-Server  | 192.168.10.22  |

    
<b>Prerequisite</b>
- A k8s-cluster or single node dosent matter
- Dhcp Server with the ability to add option 66 and 67
- Kvm/Libvirt Host

<i>In this example im using a single node k8s-cluster, so rmemeber to remove taints from the controlplane if following this, else just deploy it on a worker node.</i>
        
First of all is to install the Cli-Tools to make all of this possible
        
Install this on your K8s-Cluster controlplane Node

<br>
        
### CLi-tools and Componets
---
<b>TalosCtl </b>
     
````
sudo curl -Lo /usr/local/bin/talosctl \
"https://github.com/talos-systems/talos/releases/latest/download/talosctl-$(uname -s | tr '[:upper:]' '[:lower:]')-amd64"
chmod +x /usr/local/bin/talosctl
````
    
<b>ClusterCtl </b>
     
````
sudo curl -Lo /usr/local/bin/clusterctl \
"https://github.com/kubernetes-sigs/cluster-api/releases/download/v1.1.1/clusterctl-$(uname -s | tr '[:upper:]' '[:lower:]')-amd64" \
sudo chmod +x /usr/local/bin/clusterctl
````

    
Next up is to install the Sidero components in your K8s providing all the require Api and Crd needed

````
export SIDERO_CONTROLLER_MANAGER_HOST_NETWORK=true
export SIDERO_CONTROLLER_MANAGER_API_ENDPOINT=192.168.1.150 *
export SIDERO_CONTROLLER_MANAGER_SIDEROLINK_ENDPOINT=192.168.1.150 *

clusterctl init -b talos -c talos -i sidero
````
<i>* This must be your Controle-plane or Worker node ip, since the sidero-componentes expose themself via that ip for accessing the pxe boot and config to provide clusters from the provisioned nodes </i>

Next part is to open the services from sidero-system to for access from outside of the cluster, there are different ways to archive this, in this example ill use externalIP, this is nesswecary for following services in namespace sidero-system!
- sidero-http
- sidero-siderolink
- sidero-tftp

in extrnalIPs you define the controlplane-nodes ip from your host, to get the ip simply do

````
hostname -i
````

A easy method to add this to your services is with following command

````    
kubectl patch svc sidero-http -n sidero-system -p '{"spec": {"type": "ClusterIP", "externalIPs":["192.168.10.169"]}}'
kubectl patch svc sidero-siderolink -n sidero-system -p '{"spec": {"type": "ClusterIP", "externalIPs":["192.168.10.169"]}}'
kubectl patch svc sidero-tftp -n sidero-system -p '{"spec": {"type": "ClusterIP", "externalIPs":["192.168.10.169"]}}'
````

Now lets test if we can reach the boot file

````   
curl -I http://192.168.10.169:8081/tftp/ipxe.efi
````

 ![alt text](https://github.com/fontexD/k8s/blob/main/sidero/images/curl.png)
    
It should return a 200 http response!


### DHCP
---

Now its time to configure your DHCP server with the config for the pxe-boot nodes we are gonna use to create a Cluster, in the option field 66 and 67 add the ip which was assiged as externalIP earlier in option 66, in option 67 define the filename ```undionly.kpxe``` in my setup i use Ubiquiti DHCP as shown below, for refrence on DHCP setup from Sidero Metal https://www.sidero.dev/v0.5/getting-started/prereq-dhcp/
 ![alt text](https://github.com/fontexD/k8s/blob/main/sidero/images/ubiquiti.png)
 
### Serverclass and Environment

So when a server is created in KVM it will get the pxe-boot from our current setup, and it will be added to the default serverclass called any, but we have to tell sidero metal where the disk device is located, so we must patch the default serverclass called any and add the shown config in the red area on the imae, after spec
 ```
 kubetcl edit serverclass any
 ```
![alt text](https://github.com/fontexD/k8s/blob/main/sidero/images/serverclass.png)
 ```
  configPatches:
  - op: replace
    path: /machine/install/disk
    value: /dev/sda
 ```

Now you have to add auto-acceptance of servers so it accept em into the server-stack,¨' it can also be done by manually edit the server after its found by using 
 ```
 kubetcl get server
 kubectl edit server <server_id>
 ```
and change the field acceptet to true
 
    
Time to update the TalosOS versions from V0.14.00 (default) to V1.2.7, its used to install the node os, all fields must be 1.2.7 to change this
```
kubectl edit environment default
```
![alt text](https://github.com/fontexD/k8s/blob/main/sidero/images/environment.png)
</body>
</html>
