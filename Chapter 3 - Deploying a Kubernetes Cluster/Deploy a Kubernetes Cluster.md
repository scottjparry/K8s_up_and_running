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

Commands are being performed via minikube so kubectl is prefaced with "minikube"
> kubectl version

    Client Version: version.Info{Major:"1", Minor:"25", GitVersion:"v1.25.0", GitCommit:"a866cbe2e5bbaa01cfd5e969aa3e033f3282a8a2", GitTreeState:"clean", BuildDate:"2022-08-23T17:44:59Z", GoVersion:"go1.19", Compiler:"gc", Platform:"windows/amd64"}
    Kustomize Version: v4.5.7
    Server Version: version.Info{Major:"1", Minor:"25", GitVersion:"v1.25.0", GitCommit:"a866cbe2e5bbaa01cfd5e969aa3e033f3282a8a2", GitTreeState:"clean", BuildDate:"2022-08-23T17:38:15Z", GoVersion:"go1.19", Compiler:"gc", Platform:"linux/amd64"}
    Provides version of *kubectl* and the Kubernetes API Server

> Versions can be different. K8s tools are back/forward compatible as long as you are within 2 minor versions. Where the minor version is in the middle
>
> 1.5.2 - the Minor version is 5 in this example.

You can verify the health of the cluster overall with

> kubectl get componentstatuses

    PS C:\Users\scott> minikube kubectl get componentstatuses
    Warning: v1 ComponentStatus is deprecated in v1.19+
    NAME                 STATUS    MESSAGE                         ERROR
    controller-manager   Healthy   ok
    scheduler            Healthy   ok
    etcd-0               Healthy   {"health":"true","reason":""}

List out the nodes in the cluster.

>kubectl get nodes

    PS C:\Users\scott> minikube kubectl get nodes
    NAME       STATUS   ROLES           AGE     VERSION
    minikube   Ready    control-plane   3m46s   v1.25.0


> kubectl describe nodes node-name

    PS C:\Users\scott> minikube kubectl describe nodes minikube
    Name:               minikube
    Roles:              control-plane
    Labels:             beta.kubernetes.io/arch=amd64
                        beta.kubernetes.io/os=linux
                        kubernetes.io/arch=amd64

This also provides extensive information about the health of the node including available disk, memory and that its reporting to the K8s master correctly.

It includes capacity information

    Capacity:
    cpu:                2
    ephemeral-storage:  17784752Ki
    hugepages-2Mi:      0
    memory:             3914588Ki
    pods:               110
    Allocatable:
    cpu:                2
    ephemeral-storage:  17784752Ki
    hugepages-2Mi:      0
    memory:             3914588Ki
    pods:               110

It also contains system info and data about the pods

    Non-terminated Pods:          (7 in total)
    Namespace                   Name                                CPU Requests  CPU Limits  Memory Requests  Memory Limits  Age
    ---------                   ----                                ------------  ----------  ---------------  -------------  ---
    kube-system                 coredns-565d847f94-kwl97            100m (5%)     0 (0%)      70Mi (1%)        170Mi (4%)     4m27s
    kube-system                 etcd-minikube                       100m (5%)     0 (0%)      100Mi (2%)       0 (0%)         4m40s
    kube-system                 kube-apiserver-minikube             250m (12%)    0 (0%)      0 (0%)           0 (0%)         4m40s
    kube-system                 kube-controller-manager-minikube    200m (10%)    0 (0%)      0 (0%)           0 (0%)         4m40s
    kube-system                 kube-proxy-8j7gp                    0 (0%)        0 (0%)      0 (0%)           0 (0%)         4m27s
    kube-system                 kube-scheduler-minikube             100m (5%)     0 (0%)      0 (0%)           0 (0%)         4m40s
    kube-system                 storage-provisioner                 0 (0%)        0 (0%)      0 (0%)           0 (0%)         4m38s


# Cluster Components

Kubernetes runs a lot of components that are all managed by Kubernetes and exist within the kube-system namespace.

## Kubernetes Proxy

Responsible for routing network traffic and load balanced services into the cluster.

The proxy must be present on every nude in the cluster.

This is managed by something called a "DaemonSet" (or something else may be used) but the *kube-proxy* container should be running on all nodes in the cluster.

## Kubernetes DNS

Naming and Discovery for the services in the cluster. The DNS server runs as a replicated service on the cluster.

Depending on cluster size there may be more than 1 DNS server running in the cluster. DNS is run as a K8s deployment which manages the replicas.

/// With Kubernetes 1.12 there was a tranistion from kube-dns to core-dns as a servier. You may see kube-dns on older clusters.

The following is output

> kubectl get deployments --namespace=kube-system coredns

    PS C:\Users\scott> minikube kubectl -- get deployments --namespace kube-system coredns
    NAME      READY   UP-TO-DATE   AVAILABLE   AGE
    coredns   1/1     1            1           16m

> kubectl get services --namespace=kube-system kube-dns

    PS C:\Users\scott> minikube kubectl -- get services --namespace kube-system kube-dns
    NAME       TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)                  AGE
    kube-dns   ClusterIP   10.96.0.10   <none>        53/UDP,53/TCP,9153/TCP   16m

The DNS server IP address is shown under the *Cluster IP*. If you logged into the container you would see /etc/resolv.conf is populated with DNS.

