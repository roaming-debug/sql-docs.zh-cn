---
title: azdata bdc 参考
titleSuffix: SQL Server big data clusters
description: azdata bdc 命令的参考文章。
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 09/22/2020
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: f10ba617642d42b4b1d091374305c5f97ba1b474
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052318"
---
# <a name="azdata-bdc"></a>azdata bdc

适用于 [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

以下文章提供了 azdata 工具中 sql 命令的参考。 有关其他 azdata 命令的详细信息，请参阅 [azdata 参考](reference-azdata.md)

## <a name="commands"></a>命令

|命令|描述|
| --- | --- |
[azdata bdc spark](reference-azdata-bdc-spark.md) | Spark 命令允许用户通过创建和管理会话、语句及批处理来与 Spark 系统交互。
[azdata bdc hdfs](reference-azdata-bdc-hdfs.md) | HDFS 模块提供用于访问 HDFS 文件系统的命令。
[azdata bdc create](#azdata-bdc-create) | 创建大数据群集。
[azdata bdc delete](#azdata-bdc-delete) | 删除大数据群集。
[azdata bdc upgrade](#azdata-bdc-upgrade) | 更新部署在 SQL Server 大数据群集各个容器中的映像。
[azdata bdc config](reference-azdata-bdc-config.md) | 配置命令。
[azdata bdc endpoint](reference-azdata-bdc-endpoint.md) | 终结点命令。
[azdata bdc debug](reference-azdata-bdc-debug.md) | 调试命令。
[azdata bdc status](reference-azdata-bdc-status.md) | BDC 状态命令。
[azdata bdc control](reference-azdata-bdc-control.md) | 控制服务命令。
[azdata bdc sql](reference-azdata-bdc-sql.md) | SQL 服务命令。
[azdata bdc hdfs](reference-azdata-bdc-hdfs.md) | HDFS 服务命令。
[azdata bdc spark](reference-azdata-bdc-spark.md) | Spark 服务命令。
[azdata bdc gateway](reference-azdata-bdc-gateway.md) | 网关服务命令。
[azdata bdc app](reference-azdata-bdc-app.md) | 应用服务命令。
## <a name="azdata-bdc-create"></a>azdata bdc create
创建 SQL Server 大数据群集 - 系统中需要 Kubernetes 配置及以下环境变量 ['AZDATA_USERNAME', 'AZDATA_PASSWORD']。
```bash
azdata bdc create [--name -n] 
                  [--config-profile -c]  
                  
[--accept-eula -a]  
                  
[--node-label -l]  
                  
[--force -f]
```
### <a name="examples"></a>示例
引导式 BDC 部署体验 - 你将收到所需值的提示。
```bash
azdata bdc create
```
使用参数和经过 `azdata bdc config init` 初始化的自定义配置的配置文件实现 BDC 部署。
```bash
azdata bdc create --accept-eula yes --config-profile ./path/to/config/profile
```
指定了自定义群集名称和默认配置文件 aks dev test 的 BDC 部署。
```bash
azdata bdc create --name <cluster_name> --accept-eula yes --config-profile aks-dev-test
```
使用参数的 BDC 部署 - 由于使用了 --force 标志，不会给出任何提示。
```bash
azdata bdc create --accept-eula yes --config-profile aks-dev-test --force
```
### <a name="optional-parameters"></a>可选参数
#### `--name -n`
大数据群集名称，用于 kubernetes 命名空间。
#### `--config-profile -c`
大数据群集配置文件，用于部署群集：['openshift-prod', 'aks-dev-test-ha', 'aro-dev-test-ha', 'aks-dev-test', 'kubeadm-prod', 'aro-dev-test', 'openshift-dev-test', 'kubeadm-dev-test']
#### `--accept-eula -a`
是否接受许可条款？ [是/否]。 如果不想使用此参数，可以将环境变量 ACCEPT_EULA 设置为“yes”。 可以在 https://aka.ms/eula-azdata-en 查看 azdata 的许可条款。
#### `--node-label -l`
大数据群集节点标签，用于指定要部署到的节点。
#### `--force -f`
强制创建，系统不会提示用户输入任何值且所有问题都将作为 stderr 的一部分输出。
### <a name="global-arguments"></a>全局参数
#### `--debug`
提高日志记录详细程度以显示所有调试日志。
#### `--help -h`
显示此帮助消息并退出。
#### `--output -o`
输出格式。  允许的值：json、jsonc、table、tsv。  默认值：json。
#### `--query -q`
JMESPath 查询字符串。 请参阅 [http://jmespath.org/](http://jmespath.org)，获取详细信息和示例。
#### `--verbose`
提高日志记录详细程度。 使用 --debug 获取完整的调试日志。
## <a name="azdata-bdc-delete"></a>azdata bdc delete
删除 SQL Server 大数据群集 - 系统中需要 Kubernetes 配置。
```bash
azdata bdc delete --name -n 
                  [--force -f]
```
### <a name="examples"></a>示例
BDC 删除。
```bash
azdata bdc delete --name <cluster_name>
```
### <a name="required-parameters"></a>必需的参数
#### `--name -n`
大数据群集名称，用于 kubernetes 命名空间。
### <a name="optional-parameters"></a>可选参数
#### `--force -f`
强制删除大数据群集。
### <a name="global-arguments"></a>全局参数
#### `--debug`
提高日志记录详细程度以显示所有调试日志。
#### `--help -h`
显示此帮助消息并退出。
#### `--output -o`
输出格式。  允许的值：json、jsonc、table、tsv。  默认值：json。
#### `--query -q`
JMESPath 查询字符串。 请参阅 [http://jmespath.org/](http://jmespath.org)，获取详细信息和示例。
#### `--verbose`
提高日志记录详细程度。 使用 --debug 获取完整的调试日志。
## <a name="azdata-bdc-upgrade"></a>azdata bdc upgrade
更新部署在 SQL Server 大数据群集各个容器中的映像。 更新的映像基于传入的 docker 映像。 如果更新的映像来自与当前部署映像不同的 docker 映像存储库，则还需要“repository”参数。
```bash
azdata bdc upgrade --name -n 
                   --tag -t  
                   
[--repository -r]  
                   
[--controller-timeout -k]  
                   
[--stability-threshold -s]  
                   
[--component-timeout -p]
```
### <a name="examples"></a>示例
BDC 升级到同一存储库中的新映像标记“cu2”。
```bash
azdata bdc upgrade -t cu2
```
BDC 升级到新存储库“foo/bar/baz”中带有标记“cu2”的新映像。
```bash
azdata bdc upgrade -t cu2 -r foo/bar/baz
```
BDC 升级到同一存储库中带有标记“cu2”的新映像。升级过程将等待控制器升级，时间为 30 分钟，并等待控制器 db 升级，时间为 30 分钟。 然后，它将等待控制器和控制器 db 运行三分钟，这不会导致群集其余部分的升级发生崩溃。 升级的每个后续阶段需要 40 分钟完成。
```bash
azdata bdc upgrade -t cu2 --controller-timeout=30 --component-timeout=40 --stability-threshold=3
```
### <a name="required-parameters"></a>必需的参数
#### `--name -n`
大数据群集名称，用于 kubernetes 命名空间。
#### `--tag -t`
要将群集中的所有容器升级到的目标 docker 映像标记。
### <a name="optional-parameters"></a>可选参数
#### `--repository -r`
使群集中的所有容器都从中拉取其映像的 docker 存储库。
#### `--controller-timeout -k`
在回滚升级之前等待控制器或控制器数据库升级时所需的分钟数。
#### `--stability-threshold -s`
升级后至将其标记为稳定之前需等待的分钟数。
#### `--component-timeout -p`
暂停升级之前，等待升级的每个阶段（控制器升级后）完成所需的分钟数。
### <a name="global-arguments"></a>全局参数
#### `--debug`
提高日志记录详细程度以显示所有调试日志。
#### `--help -h`
显示此帮助消息并退出。
#### `--output -o`
输出格式。  允许的值：json、jsonc、table、tsv。  默认值：json。
#### `--query -q`
JMESPath 查询字符串。 请参阅 [http://jmespath.org/](http://jmespath.org)，获取详细信息和示例。
#### `--verbose`
提高日志记录详细程度。 使用 --debug 获取完整的调试日志。

## <a name="next-steps"></a>后续步骤

有关其他 azdata 命令的详细信息，请参阅 [azdata 参考](reference-azdata.md)。 

有关如何安装 azdata 工具的详细信息，请参阅[安装 azdata](..\install\deploy-install-azdata.md)。

