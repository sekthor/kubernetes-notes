# Container Orchestration

- A **container image** packages code, runtime and dependncies into a pre-defined format
- A **container runtime** creates containers from container images (*runC, containerd, cri-o*). The can manage containers on a single host.
- A **container orchestrator** is used to mangage containers distributed over multiple Nodes. Takes care of Fault-Tolerance and Scalability.

Container Orchestrators group systems together into clusters.
Deployment into that cluster is automated.

## Why Apps should be distrbuted across multiple Hosts

- Fault Tolerance
- On-demand Scalablity
- Optimal resource usage
- Auto-discovery to automatically discover and communicate with each other
- Accessibility from the outside world
- Seamless updates/rollbacks without any downtime

## Why use orchestrators?

Managing a couple of containers can be done manally or with a custom script.
Managing hundreds of containers is much harder to do by oneself.

Features of container orchestrators are:

- Grouping of hosts into a cluster
- Schedule containers to run on hosts based on available resources.
- Let containers communicate with each other within the cluster (regardless if they are on the same host or not)
- Bind containers and storage resources
- Group sets of similiar containers. Abstract individual containers by means of load-balancing.
- Manage/optimize resource usage
- Allow policies for secure access to applications


