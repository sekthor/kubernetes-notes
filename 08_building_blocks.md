# Building Blocks

Kubernetes has a number of Objects, that can be built.

**Labels** are key-value pairs attached to objects.
These Labels are not unique to an object.
They are used to group objects into sets.
Controllers use labels, to logically group objects.

As an example, we could have labels for `app` and `env`.

- `app:frontend` `env:dev`
- `app:backend` `env:dev`
- `app:frontend` `env:prod`
- `app:backend` `env:prod`

Controllers can use equality-based or set-based selectors to select a subset of objects.

- `equality-based`: filtering using `=`, `==` and `!=` operators
- `set-based`: filtering using `in`, `notin`, `exist`, `notexist`
    - `env in (dev,prod)` is env either dev or prod
    - `!app` objects with no label app

## Pod

A Pod is the smallest unit in K8.
It represents a single instance of the application.
It is a logical collection of containers.

- Containers of the same pod are scheduled on the same host.
- Have the same IP (network namespace).
- Have access to the same external storage (volumes).

Pods can not heal themselves.
Controllers (Deployments, ReplicaSets, ReplicationController) are in charge of replication, healing and fault-tolerance.

A Pod can be specified like this:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
  labels:
    app: nginx
spec:
  containers:
  - name: nginx
    image: nginx:1.15.11
    ports:
    - containerPort: 80
```

- `apiVersion`: must be `v1`
- `kind` specified the type of object
- `metadata` specifies the object name and label
- `spec` aka `podSpec` specifies the desired state

## Replication Controller `deprecated`

Makes sure replicas are instanciated.
Constantly compares desired state with actual state.
Pods are rarely deployed independently.
Generally, there are replicas of each pod.

It is better to use `Deployment` and specify a `ReplicaSet`.

## ReplicaSet

Deployments create ReplicaSets, which den create Pods.
The ReplicaSet is continuously checking if the current state matches the desired state.
If not the number of Pods will be changed to match it.

## Deployment

## Namespace

Namespaces are virtual sub-clusters.
To show all namespaces run:

```
kubectl get namespaces
```

Default namespaces created by K8 are:

- `kube-system`: mostly control plane agents
- `default`: objects created by administators and devs
- `kube-public`: special Namespace, readable by everyone. For exposing public info about the cluster.
- `kube-node-lease`: Node heartbeat data.

It is good practise to create additional namespaces, to isolate users, developers, apps, etc...

Resources of namespaces can be limited using `Resource Quotas` and `Limit Ranges` are used for limiting Pods or Containers in a namespace.


