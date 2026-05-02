---
slug: operators/_template
---

# {Operator Name}

## 简介

{Operator Name} 提供了 [{产品全称}]({upstream-url}) 的 Kubernetes 原生存储和管理能力。

{2-3 句话描述产品的用途、主要使用场景以及它在数据平台中的定位。}

## 前置条件

- **Kubernetes**: 1.26+
- **Kubedoop Operators**: 需要预先安装 [commons-operator](../quick-start/installation.md)、secret-operator 和 listener-operator
- **依赖组件**: {列出此产品依赖的其他 Kubedoop Operator，例如 Kafka 依赖 ZooKeeper}
- **资源要求**: {开发和测试环境的最低 CPU、内存、存储建议}

## 安装 Operator

使用 OLM（Operator Lifecycle Manager）安装 {Operator Name}：

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

详细安装步骤请参考 [快速开始](../quick-start/installation.md)。

## 配置

### Cluster CRD

{Operator Name} 通过自定义资源进行配置。以下是集群定义示例：

```yaml
apiVersion: {group}.zncdatadev.com/v1alpha1
kind: {ClusterKind}
metadata:
  name: {my-cluster}
spec:
  # 在此添加集群级别配置
```

### 角色与角色组

{Operator Name} 使用 Kubedoop 的 [角色与角色组](../core-concepts/common-configuration-mechanisms/roles-and-role-groups.md) 模型：

| 角色 | 描述 | 默认副本数 |
|------|------|:---:|
| {role-1} | {角色描述} | {n} |
| {role-2} | {角色描述} | {n} |

### 关键配置项

| 参数 | 作用域 | 描述 | 默认值 |
|------|--------|------|--------|
| {param-1} | 集群 | {描述} | {default} |
| {param-2} | 角色/角色组 | {描述} | {default} |

更多高级配置选项请参阅 [配置覆盖](../core-concepts/common-configuration-mechanisms/overrides.md)。

## 部署示例

### 最小化部署

用于测试和开发的最小化部署：

```yaml
apiVersion: {group}.zncdatadev.com/v1alpha1
kind: {ClusterKind}
metadata:
  name: {my-cluster}
spec:
  clusterConfig:
    {最小化配置}
```

### 完整部署

包含所有角色配置的生产级部署：

```yaml
apiVersion: {group}.zncdatadev.com/v1alpha1
kind: {ClusterKind}
metadata:
  name: {my-cluster}
spec:
  clusterConfig:
    {完整配置}
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

## 验证

部署完成后，验证集群是否正常运行：

```bash
# 检查集群自定义资源状态
kubectl get {resource-kind} {my-cluster} -n {namespace}

# 检查 Pod 是否正常运行
kubectl get pods -n {namespace} -l app.kubernetes.io/instance={my-cluster}

# 检查服务发现 ConfigMap
kubectl get configmap {my-cluster} -n {namespace} -o yaml
```

更多服务发现相关内容请参阅 [服务发现](../core-concepts/connectivity/service-discovery.md)。

## 清理

删除 {Operator Name} 及所有关联资源：

```bash
# 删除集群资源
kubectl delete {resource-kind} {my-cluster} -n {namespace}

# 删除 Operator 订阅（如果通过 OLM 安装）
kubectl delete subscription {operator-name}-subscription -n {namespace}
```

:::warning
删除集群资源将移除 Operator 管理的所有 Kubernetes 资源。请在操作前确保已备份重要数据。
:::

## 故障排查

常见问题和解决方案请参阅 [故障排查](../faq) 页面。

### 常见问题

| 问题 | 可能原因 | 解决方案 |
|------|---------|---------|
| Pod 卡在 `Pending` 状态 | 资源不足 | 检查节点资源，参考 [资源管理](../core-concepts/resources/resource-manage.md) |
| Pod 卡在 `CrashLoopBackOff` | 配置错误 | 查看 Pod 日志：`kubectl logs <pod-name>` |
| 连接被拒绝 | 服务未就绪 | 验证 [服务发现 ConfigMap](../core-concepts/connectivity/service-discovery.md) |

## 相关链接

- **上游项目**: [{产品全称}]({upstream-url})
- **官方文档**: [{上游文档}]({upstream-docs-url})
- **源代码**: [zncdatadev/{operator-name}-operator](https://github.com/zncdatadev/{operator-name}-operator)
- **产品镜像**: [zncdatadev/docker-images](https://github.com/zncdatadev/docker-images)
