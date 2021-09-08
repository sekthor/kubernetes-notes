# Deployment of applications

Applications can be deployed to a cluster using the web dashboard or `kubectl`.
To deploy an application to the cluster, a deployment is defined as a yaml file.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: webserver
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:alpine
        ports:
        - containerPort: 80
```

The deployment can then be created on the cluster with:

```sh
# -f specifies the source file
kubectl create -f <yaml-file>
```

Alternatively, instread of creating a service by specifying it in a `yaml` file, it can be created with the `expose` argument of `kubectl`.

```
kubectl expose deployment <deployment> --name=web-service --type=NodePort
```

## Liveness / Readiness

### Liveness

The Liveness Probe check the health of an application inside a pod.
If the health checks fail, the Pod is automatically restarted.

Liveness probes can be set with:

- liveness command
- liveness http request
- TCP liveness probe

#### Liveness command

The Pod in the example below has a `LivenessProbe` specified, that checks for the existence of a file called `/tmp/healthy` every 5 seconds.
By deafult, a `LivenessProbe` can fail three consecutive times, before the Pod is restarted.
In this example however, `failureThreshold` is specified as `1`.
It will thus be restarted after only a single failure.

```yaml
apiVersion: v1
kind: Pod
metadata:
  labels:
    test: liveness
  name: liveness-exec
spec:
  containers:
  - name: liveness
    image: k8s.gcr.io/busybox
    args:
    - /bin/sh
    - -c
    - touch /tmp/healthy; sleep 30; rm -rf /tmp/healthy; sleep 600
    livenessProbe:
      exec:
        command:
        - cat
        - /tmp/healthy
      initialDelaySeconds: 3
      failureThreshold: 1
      periodSeconds: 5
```

To monitor the container restarts, we can run `get pod` with `-w` watch option.

```
kubectl get pod <pod> -w
```

#### Liveness HTTP request

Alternatively, the kublet can send an HTTP request to the `/healthz` endpoint.

```
...
     livenessProbe:
       httpGet:
         path: /healthz
         port: 8080
         httpHeaders:
         - name: X-Custom-Header
           value: Awesome
       initialDelaySeconds: 3
       periodSeconds: 3
...
```

#### TCP liveness probe

Will try to open a TCP socket to the container running the application.

```
...
    livenessProbe:
      tcpSocket:
        port: 8080
      initialDelaySeconds: 15
      periodSeconds: 20
...
```

### Readiness

Applications may need to meet certain criteria before they are ready to serve traffic.
Such conditions are checked with a `readinessProbe`.
The below example also checks for the existence of the `/tmp/healthy` file.
The Pod will not be considered ready, until this file exists.

```
...
    readinessProbe:
          exec:
            command:
            - cat
            - /tmp/healthy
          initialDelaySeconds: 5 
          periodSeconds: 5
...
```