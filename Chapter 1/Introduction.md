# Benefits of Kubernetes

The aim of Kubernetes is to allow "reliable, scalable, distrubuted systems" to be deployed easily.

The expectation of these systems is that they are available 24/7, able to scale up and down the meet usage requirements and be reliable (recover from failures).

These requirements are all supported by using Kubernetes, Microservices and Containers.

The user of containers (and Kubernetes as an orchestration platform) can be tracked back to the requirements of Velocity, Scaling, Abstraction of Infrastructure and Efficiency.

## **Velocity**

Velocity is counted in the number of features you can ship while maintaing a highly available service. (Page 2)

While Kubernetes does not solve this problem directly, it does allow for the staged migration of containers during updates. This is managed by immutable, declaratice configurations and self-healing systems.

### **Immutability**

Traditional infrastructure is mutable, it changes over time. This is due to incremental updates being performed by staff. This includes updates to systems (apt-get update) which downloads binaries and updates them.

In the world of containers this is handled in two ways.

1. Log into container, run a command to download new software, kill the old server and start the new one.
2. Build a new container image, store it in a container registry, kill the existing container and start a new one

These two approaches may look the same but they are not.

Option 1 does not allow for a scalable approach as changes are requied each time the container is deployed.
Option 2 is preferable as you simply need to spin up a new container at is it already updated. And all containers created off the image will be up to date.

Immutable container images are a core concept for kubernetes.

Manual updates should not be performed to containers unless it is an extreme situation like a temporary repair to a mission critical production system.



### **Declarative configuraiton**


This is a concept that is discussed with distributed systems. Everything in Kubernetes is a "declarative configuration object" and represents the desired state of the system.

Declarative configuration outlined the desired state of a configuration.

> Have 3 replicaes

The opposite of this is imperative configuration.

> Create Replica A, Create Replica B, Create Replica C

Declarative configuration does not need to be executed like a script. It can be understood without being executed. As it defines the end state rather than a set of commands to be run it is less error-prone that imperative constructions.

You should find that declarative configruations are stored in a version control system. This allows for easy updates and rollbacks.

Imperative systems rarely allow you to rollback easily. The commands are build for the upgrade not the rollback.


### **Self-Healing System**

Kubernetes takes advantage of the declarative configuration and can take simple actions to try and restore a system to its desired state.

Kubernetes will initialize the system but it can also monitor it for changes. In a traditional system an administrator would need to respond to an alert to make changes. These repairs are usually expensive as they require an resource to be available to make the change. This may include being on-cal

Kubernetes can 

## Scaling

## Abstractoin Infrastructure

## Efficiency
