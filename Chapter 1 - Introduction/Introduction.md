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

Kubernetes will initialize the system but it can also monitor it for changes. In a traditional system an administrator would need to respond to an alert to make changes. These repairs are usually expensive as they require an resource to be available to make the change. This may include being on-call

Kubernetes can keep the system in its desried state. If you declare that there should be 3 replicas and a 4th one is created, Kubernetes will destory the 4th replica. Likewise if the 3rd replica is deleted Kubernetes will create a 3rd replica again.

## **Scaling**

### **Decoupling**
Scaling software systems requires decoupling your application.

This is supported by ensuring that your services can function in their own right. Everything to make an individual component run should ideally be packaged up together.


Decoupling allows you to start seperating services via APIs and load balancer.

Having a loadbalancer as a buffer allows you to scale the application without having to reconfigure it

Having an API interface allows a buffer between the implementation and the running instances of the service.

Breaking the application down into APIs allows developers to focus on small pieces of the application rather than having to treat the application as a monolith (microservices). This allows clear boundaries between application components and helps with scaling as clearly defined APIs and microservices helps to minimise communication issues during development.

Having this breakdown of microservices means that you can also scale the individual pieces of your application.

IF you suddenly start seeing incresaed web traffic because you;ve announced a new product you can scale up the web servers, but as the product is not relesaed you dont need to scale up the components of your purchasing workflow.

When the product goes live, you will probably want to scale up the purchasing workflows depending on need.

### **Scaling teams**

Scaling teams is more complex than scaling infrastructure. Within "DevOps" and arguably IT as a whole the ideal team size is the "two-pizza team" (or 6-8 people). Research indicates this size allows good knowledge sharing, fast decision making and allows a common sense of purpose to develop. It avoids the issues that impact larger teams such as hierarchy, poor visibility and infighting which will impact agility and success.

Small agile teams are able to focus on a single microservice (possibly developed as part of a larger unit) which gets implemented into a larger product.

## Abstraction of Infrastructure

Kubernetes allows the abstraction of infrastructure. This means that the underlying concepts of a system are not required knowledge to be able to consume it.

Yes, the engineer on the tools and building workflows still needs to understand the infrastruture that goes into it, but the outcome is easy-to-use self service infrastucture that developers and the business can simply consume.

Instead of knowing that you need 32 cores and 256GB of RAM you can simple ask for a "Large Web Server and a Medium Database" and have it created for you.

Defining these as code and in Kubernetes allows these to be highly portable across infrastructure, whether that is on-prem, in Microsoft Azure, Amazon Web Services, Google Cloud Platform or another CSP.

You can even move your storage away from specific storage implementations depending on the vendor with "PersistentVolumes"

The effort involved in large, but it allows a level of portability that you would struggle to get any other way.

## Efficiency

Kubernetes allows for increased efficiency, partially because developers do not need to think about machines and can simply think about applications. But also because multiple applications can all be co-located on the same machine which allows for a much denser user of the infrastructure. The alternative to this would be a virtual machine (or several) for a developer to use that has its own cost (CPU, RAM, electricity, maintenance etc).

Developers are able to create their own test environments (and terminate them) as required via kubernetes. These can be created use a kubernetes *namespace*. Alternatively all the developers might share a single cluster, the more developers that use the cluster the cheaper the cost is per container.

This can remove some of the cost associated with having multiple test environments and allow things like testing every commit made by a developer.

When you start to deploy containers instead of complete VM's incurred costs can start to decrease. Doing this with increased velocity allows you to see faster returns on the reliability of code and problem identification.

# Cliff notes

Kubernetes was built to help developers to develir features with more velocity, effficiency and agility while providing a reliable platform to do that on.

