# `kubectl` commands

## Common accronyms

```
svc: service
ns:  namespace
ctx: context
```

## `get`

The `get` argument shows information about the current namespace.
To get the requested information from all namespaces of the cluster, the `--all-namespaces` or `-A` option can be added.

```sh
# get all pods of namespace
kubectl get pods
kubectl get pods -A               # all namespaces
kubectl get pods --all-namespaces # all namespaces

# get replicasets
kubectl get replicasets

# get services
kubectl get services
```

## `create`

To create resources specified in `yaml` config files, the `create` argument can be used.
The `-f` flag specifies the source-file.

```
kubectl create -f webserver.yaml
kubectl create -f webserver-svc.yaml

kubectl create namespace
```

## `describe`

```
kubectl describe service <service>
```