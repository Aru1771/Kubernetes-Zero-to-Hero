Module 5: Aggregated ClusterRoles
==================================
Imagine your cluster installs many Custom Resource Definitions (CRDs).

Example:

Prometheus

Istio

Argo CD

Cert Manager

Each introduces new API resources.

Instead of editing the built-in view, edit, or admin ClusterRoles manually, Kubernetes supports Aggregated ClusterRoles.

Example labels:

metadata:
  labels:
    rbac.authorization.k8s.io/aggregate-to-view: "true"

Other options include:

aggregate-to-edit: "true"

aggregate-to-admin: "true"

Kubernetes automatically merges these rules into the corresponding built-in ClusterRole.

Production Example

You install Prometheus Operator.

It creates a CRD:

ServiceMonitor

You want anyone with the built-in view role to also be able to read ServiceMonitor objects.

Create:

kind: ClusterRole

metadata:
  labels:
    rbac.authorization.k8s.io/aggregate-to-view: "true"

Kubernetes automatically extends the built-in view ClusterRole with those permissions.

No manual editing required.
