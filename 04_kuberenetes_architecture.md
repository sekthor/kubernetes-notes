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

