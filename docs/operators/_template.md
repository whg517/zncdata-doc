---
slug: operators/_template
---

# {Operator Name}

## Introduction

{Operator Name} provides Kubernetes-native deployment and management for [{Product Full Name}]({upstream-url}).

{2-3 sentences describing what the product does, its primary use cases, and where it fits in a data platform.}

## Prerequisites

- **Kubernetes**: 1.26+
- **Kubedoop Operators**: [commons-operator](../quick-start/installation.md), secret-operator, and listener-operator must be installed
- **Dependencies**: {List any Kubedoop operators this product depends on, e.g., ZooKeeper for Kafka}
- **Resources**: {Minimum recommended CPU, memory, and storage for development/testing}

## Installing the Operator

Install the {Operator Name} using OLM (Operator Lifecycle Manager):

```yaml
apiVersion: operators.coreos.com/v1alpha1
kind: Subscription
metadata:
  name: {operator-name}-subscription
  namespace: {namespace}
spec:
  name: {operator-name}
  source: zncdatadev-catalog
  sourceNamespace: olm
  channel: stable
  installPlanApproval: Automatic
```

For detailed installation instructions, see the [Quick Start](../quick-start/installation.md) guide.

## Configuration

### Cluster CRD

The `{Operator Name}` is configured through a custom resource. Below is an example of the cluster definition:

```yaml
apiVersion: {group}.zncdatadev.com/v1alpha1
kind: {ClusterKind}
metadata:
  name: {my-cluster}
spec:
  # Add cluster-level configuration here
```

### Roles and Role Groups

{Operator Name} uses the Kubedoop [Roles and Role Groups](../core-concepts/common-configuration-mechanisms/roles-and-role-groups.md) model:

| Role | Description | Default Replicas |
|------|-------------|:---:|
| {role-1} | {Role description} | {n} |
| {role-2} | {Role description} | {n} |

### Key Configuration Options

| Parameter | Scope | Description | Default |
|-----------|-------|-------------|---------|
| {param-1} | Cluster | {Description} | {default} |
| {param-2} | Role/RoleGroup | {Description} | {default} |

For advanced configuration options, see [Configuration Overrides](../core-concepts/common-configuration-mechanisms/overrides.md).

## Deployment Examples

### Minimal Deployment

A minimal deployment for testing and development:

```yaml
apiVersion: {group}.zncdatadev.com/v1alpha1
kind: {ClusterKind}
metadata:
  name: {my-cluster}
spec:
  clusterConfig:
    {minimal-config}
```

### Full Deployment

A production-ready deployment with all roles configured:

```yaml
apiVersion: {group}.zncdatadev.com/v1alpha1
kind: {ClusterKind}
metadata:
  name: {my-cluster}
spec:
  clusterConfig:
    {full-config}
  roles:
    {role-1}:
      config: {}
      roleGroups:
        default:
          config: {}
    {role-2}:
      config: {}
      roleGroups:
        default:
          config: {}
```

## Verification

After deploying, verify the cluster is running:

```bash
# Check the cluster custom resource status
kubectl get {resource-kind} {my-cluster} -n {namespace}

# Check that pods are running
kubectl get pods -n {namespace} -l app.kubernetes.io/instance={my-cluster}

# Check service discovery ConfigMap
kubectl get configmap {my-cluster} -n {namespace} -o yaml
```

For more details on service discovery, see [Service Discovery](../core-concepts/connectivity/service-discovery.md).

## Cleanup

To remove {Operator Name} and all associated resources:

```bash
# Delete the cluster resource
kubectl delete {resource-kind} {my-cluster} -n {namespace}

# Delete the operator subscription (if installed via OLM)
kubectl delete subscription {operator-name}-subscription -n {namespace}
```

:::warning
Deleting the cluster resource will remove all Kubernetes resources managed by the operator. Make sure to back up any important data before proceeding.
:::

## Troubleshooting

For common issues and solutions, see the [Troubleshooting](../faq) page.

### Common Issues

| Issue | Possible Cause | Solution |
|-------|---------------|----------|
| Pods stuck in `Pending` | Insufficient resources | Check node resources and [Resource Management](../core-concepts/resources/resource-manage.md) |
| Pods stuck in `CrashLoopBackOff` | Misconfiguration | Check pod logs: `kubectl logs <pod-name>` |
| Connection refused | Service not ready | Verify the [Service Discovery ConfigMap](../core-concepts/connectivity/service-discovery.md) |

## Related Links

- **Upstream Project**: [{Product Full Name}]({upstream-url})
- **Official Documentation**: [{Upstream Docs}]({upstream-docs-url})
- **Source Code**: [zncdatadev/{operator-name}-operator](https://github.com/zncdatadev/{operator-name}-operator)
- **Product Images**: [zncdatadev/docker-images](https://github.com/zncdatadev/docker-images)
