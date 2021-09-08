# Advanced Topics

## Annotations

Annotations are used to add extra information (metadata) to Objects.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: webserver
  annotations:
    description: Deployment based PoC dates 2nd May'2019
```

They will then show up in `kubectl describe <object> <label>`

## Quota and Limits Management

ResourceQuota are used to limit computer resources.

- **Compute Resource Quota**: We can limit the total sum of compute resources (CPU, memory, etc.) that can be requested in a given Namespace.
- **Storage Resource Quota**: We can limit the total sum of storage resources (PersistentVolumeClaims, requests.storage, etc.) that can be requested.
- **Object Count Quota**: We can restrict the number of objects of a given type (pods, ConfigMaps, PersistentVolumeClaims, ReplicationControllers, Services, Secrets, etc.).

In addition, LimitRange can:

- Set compute resources usage limits per Pod or Container in a namespace.
- Set storage request limits per PersistentVolumeClaim in a namespace.
- Set a request to limit ratio for a resource in a namespace.
- Set default requests and limits and automatically inject them into Containers' environments at runtime.

## Autoscaling

To scale Kubernetes Objects automatically, instead of manually.

- **Horizontal Pod Autoscaler (HPA)**: HPA is an algorithm-based controller API resource which automatically adjusts the number of replicas in a ReplicaSet, Deployment or Replication Controller based on CPU utilization.
- **Vertical Pod Autoscaler (VPA)**: VPA automatically sets Container resource requirements (CPU and memory) in a Pod and dynamically adjusts them in runtime, based on historical utilization data, current resource availability and real-time events.
- **Cluster Autoscaler**: Cluster Autoscaler automatically re-sizes the Kubernetes cluster when there are insufficient resources available for new Pods expecting to be scheduled or when there are underutilized nodes in the cluster.

## Jobs and CronJobs

A Job creates Pods to perform a given task.
CronJob are scheduled Jobs.

## DaemonSet
