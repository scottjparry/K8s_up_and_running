# Deploying a Kubernetes Cluster

This can be deployed on bare metal or across all major public cloud providers.

## Google Kubernetes Enginer (GKE)

Requirements

- Google Cloud Platform account with billing enabled
- [gcloud](https://cloud.google.com/sdk/docs/install) tool installed

> set default zone
>
> *gcloud config set computer/zone us-west1-a*
>
> create a cluster
>
> *gcloud container clusters create kuar-cluster*
>
> get credentials for cluster
>
> *gcloud auth application-default login*


## Azure Kubernetes Service (AKS)

Requirements

- Azure Subscription
- [az CLI](https://learn.microsoft.com/en-us/cli/azure/install-azure-cli) installed
- Kubectl installed - az aks install-cli


> Create resource group
>
> *az group create --name=kuar --location=westus*
>
> Create cluster in resource group
>
> *az aks create --resource-group=kuar --name=kuar-cluster*
>
> get credentials for cluster
>
> *az aks get-credentials --resource-group=kuar --name=kuar-cluster*
>
> Ensure kubectl is installed
>
> *az aks install-cli*

## Elastic Kubernetes Service (EKS)  AWS

Requirements

- AWS Account
- [eksctl](https://docs.aws.amazon.com/eks/latest/userguide/eksctl.html) command line tool installed

> create the cluster
>
> *eksctl create cluster --name kuar-cluster*
>
> Get more information on cluster configuration
>
> *eksctl create clister --help*

## Kubernetes locally with minikube

Minikube is only suitable for local development, learning and experimentation due to limitations of running a single node.

Other features are either not supported or have limited functionality with minikube.

Requirements

- Hypervisor (Hyper-V/Virtualbox or similar)
- Install [minikube](https://minikube.sigs.k8s.io/docs/start/)

If you do not have access to a public cloud subscription to test on for any reason, Minikube would make an appropraite substitute for learning.

# The Kubernetes Client

The K8s client is kubectl - It is a command line tool for interacting with the K8s API.

It is used to explore and verify the overall health of the cluster and to manage K8s objects such as Pods, ReplicaSets and Services.

## Check cluster state

> kubectl version

Provides version of *kubectl* and the Kubernetes API Server

> Versions can be different. K8s tools are back/forward compatible as long as you are within 2 minor versions. Where the minor version is in the middle
>
> 1.5.2 - the Minor version is 5 in this example.

You can verify the health of the cluster overall with

> kubectl get componentstatuses


