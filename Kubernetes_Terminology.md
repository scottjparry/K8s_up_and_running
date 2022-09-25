This page is a compilation of terminology I have needed to look up and/or define while going through the book.

- Contexts: Contexts are used to manage different clusters. They are normally located in the homefolder/.kube/config. The config file holds info on how to find and authenticate to a cluster.
    -  Create a new config
    > kubectl config set-context my-context --namespace=mystuff
    - Change to a config
    > kubectl config use-context $context

    They can also be used to manage different clusters, or different users for those clusters depending on the flags.
- Namespace: Used to organize objects in the cluster. Similar to using folders to organise documents. Pods that service a similar function would end up in the same namespace.
- namespace isolation
- resource constraints (cgroups)
- call restrictions for container
- Dockerfile
- Registry/Remote registry
- Pods
- Nodes
- Cluster
- ReplicaSets
- Services

