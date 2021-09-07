# Authentication, Authorization, Admission Control

- `Authentication`: Logs a user in
- `Authorization`: Authorizes API requests submitted by the authenticated user
- `Admission Control`: Software modules that validate and/or modify requests.

## Authentication

Kubernetes does not store usernames.
It supports two kinds of users:

- `normal users` are managed by an external service (certificates, identity providers, etc)
- `service accounts` are used by in-cluster processes. They are tied to a particular namespace. Credentials are stored as secrets.

K8 also supports annonymous requests and user impersonation.

## Authorization

There are different Authorization modes:

- `Node`: Authorizes API requests made by Kublets.
- `Attribute-Based Access Control (ABAC)`: Policies are combines with attributes
- `Webhook`: authorization desicions are made requested from a third-party service.
- `Role-based Access Control (RBAC)`: Permissions are granted by role of user