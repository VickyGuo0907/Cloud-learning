# Kubernetes Core Concepts

List most important concepts used in Kubernetes. 

## Hardware

### Nodes

[**Nodes**](https://kubernetes.io/docs/concepts/architecture/nodes/) are worker machines in Kubernetes, which can be any devices which has CPU and RAM memory. For example a node can be anything, from smart watches, smart phones, through laptops or even a RaspberryPi. When we work with cloud providers, a node is a **virtual machine**. So a node is an abstraction over a single device.
The beatuy of this abstraction is that we don't need to know the underlying hardware structure, we will just use nodes, this way our infrastructure will be platform independent. 


### Cluster

The [**Cluster**](https://www.redhat.com/en/topics/containers/what-is-a-kubernetes-cluster) is **a group of nodes**. When you deploy programs onto the cluster, it automatically handles distribution of work to the individual nodes. If more resources are required (for example we need more memory) new nodes can be added to the cluster and the work will be redistributed automatically.
We run our code on a cluster and we should’t care on which node, the distribution of the work will be automatically handled.

### Persistent Volumes

Because our code can be relocated from one node to another (for example a node doesn’t have enough memory, so the work will be rescheduled on a different node with enough memory), data saved on a node is volatile. But there are cases when we want to save our data persistently. In this case we should use Persistent Volumes. A [**Persistent Volume**](https://kubernetes.io/docs/concepts/storage/persistent-volumes/) is like an external hard drive, you can plug it in and save you data on it.

Kubernetes was originally developed as a platform for stateless applications with persistent data stored elsewhere. As the project matured, many organizations wanted to also begin leveraging it for their stateful applications and so persistent volume management was added. Much like the early days of virtualization, database servers are not typically the first group of servers to move into this new architecture. The reason is that the database is the core of many applications and may contain valuable information so on-premises database systems still largely run in VMs or physical servers.