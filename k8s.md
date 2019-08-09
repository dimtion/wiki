# Kubernetes & microservices

## Microservices vs Monolith

- Monolith is simplier to reason about
- Monolith makes tracing and testing much easier
- Monolith works better for little projects
- Microservices _usually_ scales better
- Microservices helps with multiple team working ond differents parts
- It is _simplier_ to do partial updates in microservices when there are a lot
  of updates
- Microservices can become a big ball of mud if it is not well developed
- Microservices are enabled by Docker and would be much more complicated with VMs

## Kubernetes

Kubernetes (abr. k8s) is a container orchestration system designed for highly scalable
cloud applications. The main advantages of using k8s over historical server management systems are the following:

- Hardware agnostic: like other orchestration systems (OpenStack, Proxmox) k8s
  have a clear separation between the underlying hardware and the running
  software. The difference beeing that k8s uses Linux containers instead of
  virtual machines.
- Automatic ressources managment (cpu, storage, network)
- Various tools to help deployement, rollout...
- Logging management
- Configuration management
- Declarative state management
- Managable via an API

A nice quote that I could not find the source is:

> Kubernetes is all the perl, bash and python scripts system administrators have put together during the 30 previous years


### Quick startup using minikube

Minikube is a a tool that helps running k8s on a local machine using (by default) virtual machines to simulate the network.

Starting a minikube cluster:

```bash
minikube start
```

On Linux minikube can be used without a virtual machine drive (using Docker or another container runtime):

```bash
minikube start --vm-driver=none
```


### Vocabulary

- *Cluster*: a collection of node, with a master node and non master nodes

- *Pod*: a _Pod_ is a group of containers sharing the same context. It is
  usually a single container that contains an application. A pod can also be
  multiple containers with tightly coupled processes like a proc and its
  sidecar.
  Containers in a single Pod share the same IP address and can communicate
  between each other via `localhost` or via standard IPC.

- *Controller*: Group of replicated pods that help the management of Pods. Podss
  are usually not managed manually but through a controller. Example of
  controllers: Deployment, StatefulSet, DaemonSet

- *Node*: Physical or VM that runs Pods.

- *Service*: Abstraction over a set of pods which does service discovery and
  allow a single entry-point for an application (micro-service in the jargon).
  For compatibility with external services (external DB cluster, Object
  Storage...) it is possible to define a service _without_ a Pod selector.
- *Volume*:
- *Namespace*:

### Network

VIPs (Virtual IP Addresses) are alocated using `kube-proxy`. Since 1.2 k8s uses
iptables as the default proxying mode, but userland solution also exists.

The userland solution has the caveat of having more overhead (ring0-ring3
packet forwarding) but it has the advantage of beeing able to retry requests if
a backend pod is not available. The later problem can be fixed using `readiness
probes`.

The `iptables` proxy mode selects a backend pod randomly.

In k8s v1.11 IPVS proxy mode was implemented, it is based on netfilter hook function and so have better latency, throughput, and feature set.

IPVS documentation: https://linux.die.net/man/8/ipvsadm

### Service discovery

There is two main mode for service discovery: DNS and environement variables

Environement variables are pushed by the container runtime at pod startup with
the list of running services. For example if a `redis-master` service is
running the following variables will be created:

```bash
REDIS_MASTER_SERVICE_HOST=10.0.0.11
REDIS_MASTER_SERVICE_PORT=6379
REDIS_MASTER_PORT=tcp://10.0.0.11:6379
REDIS_MASTER_PORT_6379_TCP=tcp://10.0.0.11:6379
REDIS_MASTER_PORT_6379_TCP_PROTO=tcp
REDIS_MASTER_PORT_6379_TCP_PORT=6379
REDIS_MASTER_PORT_6379_TCP_ADDR=10.0.0.11
```

NOTE: The services must be started before the Pod creation, otherwise the
environment variables will not be populated. The problem does not exists with
DNS-based service discovery.


A headless service is a service without a VIP and no load-balancing


## Helm

Helm is a "package manager" for k8s that helps install new services on it such as Apache Spark, gitlab worker, prometheus...

* https://github.com/helm/helm
* https://helm.sh/

## Istio

Istio is a service mesh: manage services and connections between them.


## Tracing

- Zipkin
- Jagger
