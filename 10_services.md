# Services

Pods can not be assinged an direct IP address, as they are expected to potentially die.
When they are replaced by a new replica, this replica might not receive the same IP.
To solve this problem, a `service` is put in front of groups of pods.

These services operate with labels and selectors.
The pods are grouped by their labels.
This grouping is converted to a service.
A service is a sort of a loadbalancer.
It receives a clusterIP, that is only accessible from inside the cluster.

The client connects to the service using this `clusterIP`.
The service then forwards the request to one of the Pod replicas.
In the example below port `80` on the service is routed to `5000` on the Pod.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: frontend-svc
spec:
  selector:
    app: frontend
  ports:
  - protocol: TCP
    port: 80
    targetPort: 5000
```

## kube-proxy

`kube-proxy` is a daemon, that watches the API server.
It handles the addition, update, and removal of services and endpoints.
It runs on each node and configures `iptables` rules to capture clusterIP traffic and forwards it to endpoints.

## Service Discovery

There are two ways how services can be discovered.
**Environment Variables** can be used to specify IPs and ports of all active services.
`kublet` will set these variables in the Pod.
This configuration could look like the example below.

```bash
REDIS_MASTER_SERVICE_HOST=172.17.0.6
REDIS_MASTER_SERVICE_PORT=6379
REDIS_MASTER_PORT=tcp://172.17.0.6:6379
REDIS_MASTER_PORT_6379_TCP=tcp://172.17.0.6:6379
REDIS_MASTER_PORT_6379_TCP_PROTO=tcp
REDIS_MASTER_PORT_6379_TCP_PORT=6379
REDIS_MASTER_PORT_6379_TCP_ADDR=172.17.0.6
```

The second option is **DNS**.
Using an add-on, a `DNS`-entry is created for each service.
These follow a format like:

```
my-svc.my-namespace.svc.cluster.local.
```

## ServiceType

The `ServiceType` specifies the scope of a Service.
It can either be acessible

- only in the cluster
- from the outside world
- or map to an other entity in- or outside the cluster.

### ClusterIP

Is the default `ServiceType` of a `Service`.
It is a virtual IP address, only accessible inside the cluster.

### NodePort

All worker nodes map a port between `3000` and `32767` to the service.

