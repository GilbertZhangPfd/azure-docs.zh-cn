---
title: 为容器配置 Azure Monitor 代理数据收集 |Microsoft Docs
description: 本文介绍如何配置容器代理的 Azure Monitor，以控制 stdout/stderr 和环境变量日志收集。
ms.topic: conceptual
ms.date: 01/13/2020
ms.openlocfilehash: 28b93190298ae61732ff7d2e297899af4ba0e5f2
ms.sourcegitcommit: 014e916305e0225512f040543366711e466a9495
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/14/2020
ms.locfileid: "75933025"
---
# <a name="configure-agent-data-collection-for-azure-monitor-for-containers"></a>为容器 Azure Monitor 配置代理数据收集

容器 Azure Monitor 从容器化代理中收集从部署到托管 Kubernetes 群集的容器工作负载中的 stdout、stderr 和环境变量。 可以通过创建自定义 Kubernetes ConfigMaps 来配置代理数据收集设置，以控制此体验。 

本文演示如何基于要求创建 ConfigMap 和配置数据收集。

>[!NOTE]
>对于 Azure Red Hat OpenShift，将在*OpenShift-* ConfigMap 命名空间中创建模板文件。 
>

## <a name="configmap-file-settings-overview"></a>ConfigMap 文件设置概述

提供了一个模板 ConfigMap 文件，可让你轻松地使用自定义项对其进行编辑，而无需从头开始创建。 在开始之前，应查看有关[ConfigMaps](https://kubernetes.io/docs/tasks/configure-pod-container/configure-pod-configmap/)的 Kubernetes 文档，并熟悉如何创建、配置和部署 ConfigMaps。 这将允许你筛选每个命名空间或整个群集中的 stderr 和 stdout，以及在群集中的所有 pod/节点上运行的任何容器的环境变量。

>[!IMPORTANT]
>从容器工作负载收集 stdout、stderr 和环境变量所支持的最低代理版本为 ciprod06142019 或更高版本。 若要验证您的代理版本，请从 "**节点**" 选项卡中选择一个节点，然后在 "**代理图像标记**" 属性的 "属性" 窗格中说明值。 有关代理版本和每个版本中包含的内容的其他信息，请参阅[代理发行说明](https://github.com/microsoft/Docker-Provider/tree/ci_feature_prod)。

### <a name="data-collection-settings"></a>数据收集设置

下面是可以配置以控制数据收集的设置。

|密钥 |数据类型 |值 |Description |
|----|----------|------|------------|
|`schema-version` |String （区分大小写） |v1 |这是分析此 ConfigMap 时代理使用的架构版本。 当前支持的架构版本为 v1。 在计算 ConfigMap 时，不支持修改此值，将拒绝此值。|
|`config-version` |String | | 支持在源代码管理系统/存储库中跟踪此配置文件的版本。 允许的最大字符数为10，所有其他字符都将被截断。 |
|`[log_collection_settings.stdout] enabled =` |Boolean | True 或 False | 这会控制是否启用 stdout 容器日志收集。 如果设置为 `true` 并且没有为 stdout 日志收集排除命名空间（`log_collection_settings.stdout.exclude_namespaces` 设置为），则将从群集中的所有 pod/节点上的所有容器中收集 stdout 日志。 如果未在 ConfigMaps 中指定，则默认值为 `enabled = true`。 |
|`[log_collection_settings.stdout] exclude_namespaces =`|String | 逗号分隔的数组 |将不收集其 stdout 日志的 Kubernetes 命名空间的数组。 仅当 `log_collection_settings.stdout.enabled` 设置为 `true`时，此设置才有效。 如果未在 ConfigMap 中指定，则默认值为 `exclude_namespaces = ["kube-system"]`。|
|`[log_collection_settings.stderr] enabled =` |Boolean | True 或 False |这会控制是否已启用 stderr 容器日志收集。 如果设置为 `true` 并且没有为 stdout 日志收集（`log_collection_settings.stderr.exclude_namespaces` 设置）排除命名空间，则将从群集中的所有 pod/节点上的所有容器中收集 stderr 日志。 如果未在 ConfigMaps 中指定，则默认值为 `enabled = true`。 |
|`[log_collection_settings.stderr] exclude_namespaces =` |String |逗号分隔的数组 |将不收集 stderr 日志的 Kubernetes 命名空间的数组。 仅当 `log_collection_settings.stdout.enabled` 设置为 `true`时，此设置才有效。 如果未在 ConfigMap 中指定，则默认值为 `exclude_namespaces = ["kube-system"]`。 |
| `[log_collection_settings.env_var] enabled =` |Boolean | True 或 False | 此设置控制群集中的所有 pod/节点上的环境变量集合，如果 ConfigMaps 中未指定，则默认为 `enabled = true`。 如果已全局启用了环境变量的集合，则可以通过将环境变量设置 `AZMON_COLLECT_ENV` 为 False，将环境变量设置为**False** ，方法是使用 Dockerfile 设置，或在**env：** 节下的[Pod 的配置文件](https://kubernetes.io/docs/tasks/inject-data-application/define-environment-variable-container/)中。 如果已全局禁用环境变量的集合，则无法为特定容器启用收集（也就是说，可在容器级别应用的唯一重写是在全局启用时禁用集合）。 |
| `[log_collection_settings.enrich_container_logs] enabled =` |Boolean | True 或 False | 此设置控制容器日志扩充，以填充群集中所有容器日志的每个日志记录的名称和图像属性值。 如果 ConfigMap 中未指定，则默认为 `enabled = false`。 |

ConfigMaps 是一个全局列表，只能有一个 ConfigMap 应用于代理。 不能将其他 ConfigMaps overruling 到集合中。

## <a name="configure-and-deploy-configmaps"></a>配置和部署 ConfigMaps

执行以下步骤，配置 ConfigMap 配置文件并将其部署到群集。

1. [下载](https://github.com/microsoft/OMS-docker/blob/ci_feature_prod/Kubernetes/container-azm-ms-agentconfig.yaml)模板 ConfigMap yaml 文件并将其另存为容器 azm-agentconfig. yaml。 

   >[!NOTE]
   >使用 Azure Red Hat OpenShift 时，此步骤不是必需的，因为群集中已存在 ConfigMap 模板。

2. 编辑包含自定义项的 ConfigMap yaml 文件，以收集 stdout、stderr 和/或环境变量。 如果正在编辑 ConfigMap yaml file for Azure Red Hat OpenShift，请首先运行命令 `oc edit configmaps container-azm-ms-agentconfig -n openshift-azure-logging` 在文本编辑器中打开该文件。

    - 若要排除 stdout 日志收集的特定命名空间，可以使用以下示例配置键/值： `[log_collection_settings.stdout] enabled = true exclude_namespaces = ["my-namespace-1", "my-namespace-2"]`。
    
    - 若要禁用特定容器的环境变量集合，请将键/值 `[log_collection_settings.env_var] enabled = true` 设置为全局启用变量集合，然后按照[此处](container-insights-manage-agent.md#how-to-disable-environment-variable-collection-on-a-container)的步骤完成特定容器的配置。
    
    - 若要禁用 stderr 日志收集群集，可以使用以下示例配置密钥/值： `[log_collection_settings.stderr] enabled = false`。

3. 对于除 Azure Red Hat OpenShift 之外的群集，请通过运行以下 kubectl 命令来创建 ConfigMap： `kubectl apply -f <configmap_yaml_file.yaml>` 在除 Azure Red Hat OpenShift 以外的其他群集上。 
    
    示例：`kubectl apply -f container-azm-ms-agentconfig.yaml`。 

    对于 Azure Red Hat OpenShift，请保存在编辑器中所做的更改。

在生效之前，配置更改可能需要几分钟才能完成，并且群集中的所有 omsagent 箱都将重新启动。 重新启动是所有 omsagent pod 的滚动重启，而不是同时重新启动。 重新启动完成后，会显示一条类似于以下内容的消息，其中包括 result： `configmap "container-azm-ms-agentconfig" created`。

## <a name="verify-configuration"></a>验证配置

若要验证配置是否已成功应用于 Azure Red Hat OpenShift 以外的群集，请使用以下命令从代理 pod 查看日志： `kubectl logs omsagent-fdf58 -n=kube-system`。 如果 omsagent pod 出现配置错误，输出将显示类似于以下内容的错误：

``` 
***************Start Config Processing******************** 
config::unsupported/missing config schema version - 'v21' , using defaults
```

还可以查看与应用配置更改相关的错误。 下列选项可用于对配置更改执行额外的故障排除：

- 使用同一个 `kubectl logs` 命令从代理 pod 日志。 

    >[!NOTE]
    >此命令不适用于 Azure Red Hat OpenShift 群集。
    > 

- 从实时日志。 实时日志显示类似于以下内容的错误：

    ```
    config::error::Exception while parsing config map for log collection/env variable settings: \nparse error on value \"$\" ($end), using defaults, please check config map for errors
    ```

- Log Analytics 工作区中的**KubeMonAgentEvents**表。 数据每小时发送一次，并出现配置错误的*错误*严重性。 如果没有错误，则表中的条目将包含带有严重性*信息*的数据，而不报告任何错误。 **Tags**属性包含有关在其上发生错误的 pod 和容器 ID 的详细信息，以及最后一个小时内的第一个匹配项和最后一个匹配项。

- 使用 Azure Red Hat OpenShift，通过搜索**ContainerLog**表来检查 omsagent 日志，以验证是否已启用 OpenShift 日志记录收集。

更正了除 Azure Red Hat OpenShift 之外的群集上的 ConfigMap 中的错误后，请通过运行以下命令来保存 yaml 文件并应用更新的 ConfigMaps： `kubectl apply -f <configmap_yaml_file.yaml`。 对于 Azure Red Hat OpenShift，请通过运行以下命令来编辑和保存更新的 ConfigMaps：

``` bash
oc edit configmaps container-azm-ms-agentconfig -n openshift-azure-logging
```

## <a name="applying-updated-configmap"></a>应用更新的 ConfigMap

如果已在 Azure Red Hat OpenShift 以外的群集上部署了 ConfigMap，并希望使用较新的配置对其进行更新，则可以编辑以前使用过的 ConfigMap 文件，然后使用与前面相同的命令 `kubectl apply -f <configmap_yaml_file.yaml`。 对于 Azure Red Hat OpenShift，请通过运行以下命令来编辑和保存更新的 ConfigMaps：

``` bash
oc edit configmaps container-azm-ms-agentconfig -n openshift-azure-logging
```

在生效之前，配置更改可能需要几分钟才能完成，并且群集中的所有 omsagent 箱都将重新启动。 重新启动是所有 omsagent pod 的滚动重启，而不是同时重新启动。 重新启动完成后，会显示一条类似于以下内容的消息，其中包括 result： `configmap "container-azm-ms-agentconfig" updated`。

## <a name="verifying-schema-version"></a>验证架构版本

支持的配置架构版本在 omsagent pod 上以 pod 注释（架构版本）的形式提供。 可以通过以下 kubectl 命令查看它们： `kubectl describe pod omsagent-fdf58 -n=kube-system`

输出将显示类似于以下内容的注释架构版本：

```
    Name:           omsagent-fdf58
    Namespace:      kube-system
    Node:           aks-agentpool-95673144-0/10.240.0.4
    Start Time:     Mon, 10 Jun 2019 15:01:03 -0700
    Labels:         controller-revision-hash=589cc7785d
                    dsName=omsagent-ds
                    pod-template-generation=1
    Annotations:    agentVersion=1.10.0.1
                  dockerProviderVersion=5.0.0-0
                    schema-versions=v1 
```

## <a name="next-steps"></a>后续步骤

- 容器 Azure Monitor 不包含一组预定义的警报。 查看[使用容器 Azure Monitor 创建性能警报](container-insights-alerts.md)，了解如何为高 CPU 和内存使用率创建建议警报，以支持 DevOps 或操作过程和过程。

- 启用监视功能可收集 AKS 或混合群集的运行状况和资源利用率，以及在这些群集上运行的工作负荷，了解[如何使用](container-insights-analyze.md)容器 Azure Monitor。

- 查看[日志查询示例](container-insights-log-search.md#search-logs-to-analyze-data)，了解预定义的查询和示例，以便对群集进行警报、可视化或分析。
