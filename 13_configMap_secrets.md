# ConfigMap and Secrets

## ConfigMap

`ConfigMap` is a key-value store for configuration information.
It decouples this information from the container image.
`ConfigMap`s are created with `kubectl` or as a `yaml` object.

```
kubectl create configmap <name> --from-literal=key1=value1 --from-literal=key2=value2
```

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: customer1
data:
  TEXT1: Customer1_Company
  TEXT2: Welcomes You
  COMPANY: Customer1 Company Technology Pct. Ltd.
```

For the `yaml` version, we run the `kubectl create -f <yaml-file>` command.

### Using ConfigMaps in Pods

Values of a `ConfigMap` can be used in a container as environment variables.

```yaml
...
  containers:
  - name: myapp-full-container
    image: myapp
    envFrom:
    - configMapRef:
      name: full-config-map
...
```

From several config maps:

```yaml
...
  containers:
  - name: myapp-specific-container
    image: myapp
    env:
    - name: SPECIFIC_ENV_VAR1
      valueFrom:
        configMapKeyRef:
          name: config-map-1
          key: SPECIFIC_DATA
    - name: SPECIFIC_ENV_VAR2
      valueFrom:
        configMapKeyRef:
          name: config-map-2
          key: SPECIFIC_INFO
...
```

Alternatively, values can be accessed as Volumes.
For each key-value pair, a file iscreated, named like the key and containing the value.

```yaml
...
  containers:
  - name: myapp-vol-container
    image: myapp
    volumeMounts:
    - name: config-volume
      mountPath: /etc/config
  volumes:
  - name: config-volume
    configMap:
      name: vol-config-map
...
```

## Secrets

Secrets are for sensitive information such as passwords, tokens, etc.
Secrets are stored in plain text in `etcd`.
Therefore, access to `ectd` must be limited.
Encryption of secrets is possible.

Creating a secret from literal:

```
kubectl create secret generic my-password --from-literal=password=mysqlpassword
```

Both `get` and `describe` will not reveal the value of the secret.

```
kubectl get secret my-password
kubectl describe secret my-password
```

Creating a secret from a `yaml` object.

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: my-password
type: Opaque
data:
  password: bXlzcWxwYXNzd29yZAo=
```

```
kubectl create -f <secret yaml file>
```

### Using secrets in Pods

They can be usd as environment variables.

```yaml
....
spec:
  containers:
  - image: wordpress:4.7.3-apache
    name: wordpress
    env:
    - name: WORDPRESS_DB_PASSWORD
      valueFrom:
        secretKeyRef:
          name: my-password
          key: password
....
```

Or they can be used as files:

```yaml
....
spec:
  containers:
  - image: wordpress:4.7.3-apache
    name: wordpress
    volumeMounts:
    - name: secret-volume
      mountPath: "/etc/secret-data"
      readOnly: true
  volumes:
  - name: secret-volume
    secret:
      secretName: my-password
....
```
