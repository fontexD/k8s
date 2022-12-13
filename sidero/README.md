<!DOCTYPE html>
<link rel="stylesheet" type="text/css" href="https://fonts.googleapis.com/css?family=Dosis&display=swap">
<html>
    <body>


## Sidero Metal - Automation at the next level


<i>What ive used in this Article</i>

| Application | Version |
| ------------- | ------------- |
| Kubernetes  | V1.24  |
| CentOS Stream  | V9.0  |
| Unifi DHCP| V7.0.2 |
| Libvirt/Kvm| V8.10.0 |

<br>
<b>This</b> is by far one of the best way to archive automation of K8s deployment and life cycle mangement ive come to experience so far, i used to do Terraform to provision VM 's and Ansible for configuration of the VM's, which is also and incredible nice way to do things, but it does require some maintanence and understandig of Ansible , Terraform and the whole mangement part of this including you have to spend time keeping your OS for every node up to date, installing required packages, making sure there is no broken parts on your os, all this taking your time from atually using k8s and not just keeping it alive and up to date!

## So what is Sidero Metal and TalosOs? 

### Sidero Metal
Sidero (“Iron” in Greek) is a project created by the Sidero Labs team. Sidero Metal provides lightweight, composable tools that can be used to create bare-metal Talos Linux + Kubernetes clusters. These tools are built around the Cluster API project.

Because of the design of Cluster API, there is inherently a “chicken and egg” problem: you need an existing Kubernetes cluster in order to provision the management plane, that can then provision more clusters. The initial management plane cluster that runs the Sidero Metal provider does not need to be based on Talos Linux - although it is recommended for security and stability reasons. The Getting Started guide will walk you through installing Sidero Metal either on an existing cluster, or by quickly creating a docker based cluster used to bootstrap the process

// Description from Sidero Metal Site https://www.sidero.dev/v0.5/overview/introduction/


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

// Description from Talos Os Site h[ttps://www.sidero.dev/v0.5/overview/introduction/](https://www.talos.dev/v1.2/introduction/what-is-talos/)
<br>
<br>
        
## Lets get onto it!

        <b> Prerequisite </b>
- A k8s-cluster or single node dosent matter
- Dhcp Server witht the ability to add option 66 and 67
- Kvm/Libvirt Host
        
    </body>
</html>
