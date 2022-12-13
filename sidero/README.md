
## Sidero Metal - Automation at the next level


<i>What ive used in this Article</i>

| Application | Version |
| ------------- | ------------- |
| Kubernetes  | 1.24.0  |
| CentOS Stream  | 9.0  |

<br>
<b>This</b> is by far one of the best way to archive automation of K8s deployment and life cycle mangement ive come to experience so far, i used to do Terraform to provision VM 's and Ansible for configuration of the VM's, which is also and incredible nice way to do things, but it does require some maintanence and understandig of Ansible , Terraform and the whole mangement part of this including you have to spend time keeping your OS for every node up to date, installing required packages, making sure there is no broken parts on your os, all this taking your time from atually using k8s and not just keeping it alive and up to date!

## So what is Sidero Metal and TalosOs? 

### Sidero Metal
Sidero (“Iron” in Greek) is a project created by the Sidero Labs team. Sidero Metal provides lightweight, composable tools that can be used to create bare-metal Talos Linux + Kubernetes clusters. These tools are built around the Cluster API project.

Because of the design of Cluster API, there is inherently a “chicken and egg” problem: you need an existing Kubernetes cluster in order to provision the management plane, that can then provision more clusters. The initial management plane cluster that runs the Sidero Metal provider does not need to be based on Talos Linux - although it is recommended for security and stability reasons. The Getting Started guide will walk you through installing Sidero Metal either on an existing cluster, or by quickly creating a docker based cluster used to bootstrap the process

// Description from Sidero MetalOwn Sites https://www.sidero.dev/v0.5/overview/introduction/
