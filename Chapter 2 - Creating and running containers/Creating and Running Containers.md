# Why containers?

Applications are typically comprised of a language runtime, various libraries and the source code.

Libraries may require external libraries such as libssl to run. These libraries are usually shipped with the OS that is installed on a particular machine.

If the app has a dependency on a library that is installed on the developers workstation but not on the application server you get the "but it runs on my machine" error when trying to deploy code.

A program can only only be considred reliably when it can be reliably deployed to any system and function as expected.

When doing an imperative deployment you are at an increased risk of running into these problems because you will have a workflow like below

>1. Connect to remote machine
>2. Download source code from location A
>3. Patch Components 1 -3
>4. Update Library F
>5. Install source code
>6. Run application

If anything in the above steps changes from the time the developer writes the code to when the application is installed you may have problems.

This can include updates to the libraries you are installing and just creates needless complexity in the deployment process.

Container images are largely based on layers.

With additional layers adding, removing or modifying files from the preceding layer.

>- Container A - Base OS (Debian)
>    - Container B: Build upon A, by adding Ruby v2.1.10
>    - Container C: Build upon A, by Adding Golang v1.6

As you can see above, there are now 3 containers, A, B and C.
Containers B and C are forked from A.
B and C container the same base container files but have Rails or Golang added.

Continuing on from above

You can build a container that allows support for Rails v3 or v4 in the future while still maintaing existing containers.

>- Container A - Base OS (Debian)
>    - Container B: Build upon A, adding Ruby 2.1.10
>        - Container D: Build upon B, by adding Rails v4.2.6
>        - Container E: Build ipon B, by adding Rails v3.2.x
>    - Container C: Build upon A, by Adding Golang v1.6

These container images typically come with a container configuration file. It provides instructions on how to setup the container environment and execute an application entry point.

This would include things such as
- Setting up networking
- namespace isolation
- resource constraints
- call restrictions for container

Containers fall into two main categories

System containers: These seek to mimic a VM, often run full boot processes and include system services typically found in VM's (ssh, cron, syslog)

Application containers: Commonly run a single application. It provides a level of granularity for composing scalable applications that can be leveraged by pods.


# Building Application Images with Docker

This will be largely focused on application containers rather than system containers.

## Dockerfiles

A "Dockerfile" can be used to automate the creation of a Docker image.

Example from book is below

> FROM alpine
>
> MAINTAINER Kelsey Hightower <kelsey.hightower@kuar.io>
>
> COPY bin/kuard /kuard
>
> ENTRYPOINT ["/kuard"]

The above text would be stored in a text file, typically named *Dockerfile*

This provides instructions to add the "kuard" binary to the docker image.

Building the docker image is done with the below command

> docker build -t kuard-amd64:1

This adds the image to the local docker registry, making it accessible to a single machine only. This image can be saved in a container registry to allow wider sharing of the image.

## Image Security

The security of container images is critical and it should be treated as such.

Make sure that you are following all best bpractices for packaging a distributing applications when building for kubernetes.

Things to be aware of

- Build containers with psaswords baked in (at any layer of the image)
- Deleting sensitive data from a single layer
    - The data can still be present in other layers.

You should never mix secrets and images. EVER. There are plenty of enterprise grade solutions for storing secrets and accessing them from applications. Use those instead.

## Optimizing image sizes

Files that are removed from subsequent layers of an image are actually still present in the preceding layers.

> - Layer A: Contains 'BigFile'
>     - Layer B: removes 'BigFile'
>         - Layer C: Builds on B, by adding static binary

'BigFile' is still present in the container image. It is part of Layer A, it just stopped being accessible as part of Layer B.

The data is still included and transmitted across the network.

Solution: Remove 'BigFile' from Layer A


Remember that each layer is an independent delta to the layer below it.

Every time you change a layer, it changes the layer that comes after it. This means that preceiding layers may need to be rebuilt, repushed and repulled to deploy an image.

> Example 1
> - Layer A: Contains base OS
>    - Layer B: adds source code server.js
>        - Layer C: installed the 'node' package

compared to

> Example 2
> - Layer A: Contains base OS
>    - Layer B: installed the 'node' package
>        - Layer C: adds source code server.js

Initially both of these images would function the same.

When server.js changes and needs to be updated the behaviour changes significantly.

In Example 1, the source code would change, Layer B is updated with new source code, Layer C then needs to be updated as well.

In Example 2, the source could would change in Layer C, however Layer B would not need to be updated as the 'node' package has not changed.

In general layers should be ordered from "Least likely to be changed" to "Most likely to be changed" this avoids having to make extra changes every time an update is made.

