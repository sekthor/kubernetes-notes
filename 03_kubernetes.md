# Kubernetes

## Features

- **Automatic bin packaging**: automatically schedule containers based on resource needs/constraints. Maximize utilization, whitout sacrificing availablilty.
- **Self-healing**: Replace contianers on failed nodes. Kill & Restart unresponsive containers. Prevents traffic to unresponsive containers.
- **Horizontal Scaling**: Apps are scaled manually or automatically based on CPU or other metrics.
- **Service Discovery/load balancing**: Each container has its own IP. Sets of containers have a DNS name -> load balanacing.
- **automated rollouts/rollbacks** Seamless rollout of application updates. Constant monitoring of health.
- **Secret and Configuration Management**: Secretes are managed independently from images.
- **Stroage orchestration**: Automatically mounts Software defined storage (SDS).
- **Batch execution**: batch execution, long tasks, replaces failed containers.

## Why Kubernetes?

- portable
- exentisble
- local & remote machines
- bare metal & virtual machines
- modular
