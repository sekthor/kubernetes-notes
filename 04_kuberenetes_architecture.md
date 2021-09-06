# Kubernetes Architecture

Kubernetes works with 1 or more **master nodes** and 1 or more **worker nodes**.

## Master Node

The master node is the running environment for the **control plane**, which manages the cluster.
The control plane needs to be kept running at all cost, loosing it means downtime.
To prevent that, the master node can be replicated (High Availability Mode (HA)).
One master node is managing the cluster, and syncing itself with the others.
Should this node fail, one of the redundant masters can take over.

The cluser state is persisted in **etcd**.
It is a key-value store for cluster state related data.

The control plane consists of:

- API Server
- Scheduler
- Controller MAnagers
- Data Store

In addition, the master node also runs:

- Container Runtime
- Node Agenet
- Proxy

### API Server

Administrative task are handled by the `kube-apiserver`.
It intercepts REST-calls from outside, then validates and processes them.
It reads the current cluster state from `etcd` and after execution, writes the new state.

The API Server is the only component to talk to `etcd`.
All other control plane comonents talk to the API Server, when they need information from etcd.

### Scheduler

The scheduler assigns pods to nodes.
An algorithm chooses the node.
Constraints may be set by users (disk==ssd).

### Controller Manager

A watcher, that continuously compare the desired cluster state with its current state.
Mismatches result in corrective actions.

- `kube-controller-manager` acts when a node is down, ensures pod counts are as expected, creates endpoints, services accounts and API access tokens.
- `cloud-controller-manager` runs controllers for infrastructure of cloud provider.

### etcd

`etcd` persits cluster states in a key-value store.
Data is appended, never overwritten.
Obsolete data is compacted.

`etcdctl` is a cli tool with capacbilties for backups, snapshots and restoring.

`etcd` can be used in two different ways:

- `stacked etcd` runs `etcd` on the master nodes with the same resources as the other control plane components.
- `external etcd` isolates the etcd data from other control plane components to and external etcd cluster

One of the `etcd` instances is the master, receiving all changes.
The others are followers.
They synchronize using the `Raft Consensus Algorithm`.

## Worker Nodes

Pods are scheduled on worker nodes (by Master Node).
A Pod is the smallest unit in K8.
It is a logical collection of one or more containers scheduled together.

A worker node contains of:

- Container runtime
- Node Agent (kubelet)
- Proxy (kube-proxy)
- Addons for: DNS, Dashboard UI, Monitoring, Logging

### Container Runtime

K8 can not run containers itself.
A container runtime is used, to manage a containers lifcycle.
This runtime needs to be installed on the worker node, the Pod is to be scheduled.

Supported runtimes include

- (Docker)
- CRI-O
- containerd
- frakti

### Node Agent (kublet)

The Node Agent (`kubelet`) runs on every worker node and comunicates with the control plane.
The kublet interacts with the container runtime and monitors health and resources.
A *CRI shim* (container runtime interface) application, acts as a middleware between kublet and container runtime and abstracts the runtime to a common interface, the kublet can access.
The API Server sends Pod definitions to the kublet.

### kube-proxy

Updates and maintains networking rules.
It forwards connection requests to Pods.

### Addons

Addons are features that are not supported by Kubernetes itself and thus implemented as third-party addon.
