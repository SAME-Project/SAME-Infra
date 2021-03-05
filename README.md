# SAME-Infrastructure

## Handy Terraform / K3S Resources:

I found some basic guides around how to do K3S + Terraform. These can be the basis for a basic 5 part setup.

**Launching a Terraform AKS cluster**  
https://build5nines.com/terraform-create-an-aks-cluster/

**Launching a Terraform AWS + K3S Cluster**   
https://github.com/rancher/terraform-k3s-aws-cluster

**Terraform module for GKE**  
https://github.com/spotify/terraform-gke-kubeflow-cluster

**Installing KubeFlow via Terraform**  
https://github.com/datarootsio/terraform-module-kubeflow

**Setting up a vSphere VM using K3S + Terraform**  
https://www.virtualthoughts.co.uk/2020/08/14/automated-deployment-of-k3s-and-rancher-on-vsphere-with-terraform/

**Pre-created / existing GitHub library for using Terraform to spin up K3S on various clusters**  
https://github.com/xunleii/terraform-module-k3s

**Simple, 2-Node K3S Cluster on Terraform (Supports Arm), slightly out of date**  
https://github.com/wjimenez5271/k3s-terraform

**Tutorial by Alex Ellis on how to run K3S via Terraform using Equinix Metal (Baremetal)**  
https://blog.alexellis.io/bare-metal-kubernetes-with-k3s/

# Transportable K3S AI

## Project Overview

Today, Data Scientists wanting to focus on what they do best are forced to deal with many inconveniences when it comes to setting up, stabilizing and sharing their working environments. This makes it challenging to easy organize, configure, transport and share their work.

Fortunately, Terraform (a technology to enable simplification of developer infrastructure) offers solutions to help streamline these challenges. This project seeks to leverage Terraform to create a standardized way to deploy a Kubernetes environment such that Kubeflow (AI training on Kubernetes) or any other Kubernetes based ML workloads can be deployed declaratively to any environment (local, on a VM or in *any* cloud) (NOTE: Actual deployment of that workload is out of scope for this project - it will be handled elsewhere). Providing this consistent, declarative setup system will unleash the productivity of Data Scientists and their teams. 

## Ultimate Vision

**“With a single command, a Data Scientist can set up, provision and run a reproducible, reusable ML development environment.”**

## Basic Design Considerations
Terraform will be used to enable IaaS on all target environments (cloud, local)
We plan to use a single VM to ensure environment is fully transportable from local to cloud
If a data scientist needs a multiple VM setup, they should deploy to a k8s cluster
K3S will be used for initial launch
K3S has the benefit of needing no global permissions if scoped 
K3S can run anywhere (local, VM, cloud) no problem via Terraform
Anything non-portable that cannot be declaratively deployed to both a laptop and a VM in the cloud should be tracked and punted to v2

## Technical Objectives
By setting up this consistent API
Anyone can run KubeFlow on any cloud or at the edge via consistent, simple provisioning of a k8s API (either via k3s or hosted k8s)
Run KubeFlow locally, on a laptop or workstation, dedicated training rig - as easily as ‘pip install jupyter’
Seamlessly transport AI training from edge device to cloud as needed

However setup of platforms on top of the k8s API is another project’s scope.

## Project Roadmap
- Define and document project scope & deliverables, key personnel and objectives 
- Set up and refine a shared GitHub repo (project will be separate from Kubeflow)
- Create Terraform configurations to support basic launch configurations:

**P0:**
K3S Local
K3S (VM)

**P1 - extend the same code to support standardized deployments of..** 
- AKS (Azure) 
- GKS (GCP) 
- EKS (AWS) 

## Near Term Gaps (The purpose of this document)
- Collection of terraform (one per provider) scripts for setting up:
- A VM on the target clouds 
- K3s on those VMs
- Doing both in such a way that maximizes shared code and parameterization via environment settings
- Testing using the Porter CNAB to deploy KF to those deployments (proof of concept from Bernd Verst and Ralph Squillace (porter install -c kubeflow --tag ghcr.io/squillace/aks-kubeflow:v0.3.1 --param AZURE_RESOURCE_GROUP=winlin --param CLUSTER_NAME=kubeflow`)

## Nice to have: 
- Common pattern for TF scripts for setting up K8s on the hosted provider
 (these exist but are often in very different formats/styles)
- if we could fork and centralize and share code (with a similar pattern as the k3s work), that would be hugely valuable.

## Target Local,Cloud & Owners
- Google Cloud Platform - Gabe Weiss 
- Amazon Web Services - Jeremy Wallace (?)
- Microsoft Azure - Paul DeCarlo
- Nuno De Carmo (https://twitter.com/nunixtech)
- Ralph Squillace  (https://twitter.com/ralph_squillace) 
- Alessandro Festa (https://twitter.com/bringyourownai)
- Gabriele Santomaggio (https://twitter.com/GSantomaggio)
- NVIDIA - Rex St. John

## Supported Environments
- K3S Local
- K3S (VM)
- AKS (Azure)
- GKS (GCP)
- EKS (AWS)

## Need Help With…
- K3s setup via TF
- VM setup via TF
- What do we need to do about additional resources - nothing?
...

## Link: 
- https://github.com/kubernauts/bonsai
- https://twitter.com/ralph_squillace/status/1360646969258090499

## Job to be done:
I am a data scientist who may or may not have access to provision a full cluster. I want to set up Kubeflow in a reliable way and so to get there, I’m either going to declaratively set up a hosted K8s cluster on the provider of my choice or set up a k3s “cluster) on a single beefy VM or my laptop - this consistently provided k8s API will let me deploy Kubeflow (OUT OF SCOPE FOR THIS WORK) and get going in a production ready way quickly without understanding any of the details for Kubeflow or k8s

## Our theory -
- Make it easy to run Kubernetes (K3S preferred or K8s) on 5+ infrastructure platforms via Terraform
- Then install KubeFlow to that
- Then party on ML (as a Data Scientist who doesn't want to do setting up all the things)

By supporting k3s as easily as hosted k8s, we make swapping out from k3s to k8s a single environment setting.
By separating the terraform installation of a Kubernetes API from the kubeflow installation, it enables us to move to multi cloud Reproducibility much easier.

## UX:

### Kubeflow installed on laptop 
```
$ export K8S_PROVIDER=local-k3s 
$ terraform plan; terraform apply 
$ porter build Kubeflow; porter install 
```
### Kubeflow installed on k3s VM
```
$ export K8S_PROVIDER=vm-k3s
$ export K8S_PROVIDER_ADDRESS=vm7382.contoso.com
$ terraform plan; terraform apply
$ porter build Kubeflow; porter install 
```
### Kubeflow installed on AKS cluster
```
$ export K8S_PROVIDER=aks
$ terraform plan; terraform apply
$ porter build Kubeflow; porter install 
```





