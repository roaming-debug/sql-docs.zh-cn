---
title: azdata arc dc reference
titleSuffix: SQL Server big data clusters
description: azdata arc dc 命令的参考文件。
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 09/22/2020
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: b223b3b69ba3eda1be5f882b4cabd868c345b20c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100049007"
---
# <a name="azdata-arc-dc"></a>azdata arc dc

适用于 [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

以下文章提供了 azdata 工具中 sql 命令的参考。 有关其他 azdata 命令的详细信息，请参阅 [azdata 参考](reference-azdata.md)

## <a name="commands"></a>命令

|命令|说明|
| --- | --- |
[azdata arc dc create](#azdata-arc-dc-create) | 创建数据控制器。
[azdata arc dc delete](#azdata-arc-dc-delete) | 删除数据控制器。
[azdata arc dc endpoint](reference-azdata-arc-dc-endpoint.md) | 终结点命令。
[azdata arc dc status](reference-azdata-arc-dc-status.md) | 状态命令。
[azdata arc dc config](reference-azdata-arc-dc-config.md) | 配置命令。
[azdata arc dc debug](reference-azdata-arc-dc-debug.md) | 调试命令。
[azdata arc dc export](#azdata-arc-dc-export) | 导出指标、日志或使用情况。
[azdata arc dc upload](#azdata-arc-dc-upload) | 上传导出的数据文件。
## <a name="azdata-arc-dc-create"></a>azdata arc dc create
创建数据控制器 - 系统中需要 kube 配置以及以下环境变量 ['AZDATA_USERNAME', 'AZDATA_PASSWORD']。
```bash
azdata arc dc create --namespace -ns 
                     --name -n  
                     
--connectivity-mode  
                     
--resource-group -g  
                     
--location -l  
                     
--subscription -s  
                     
[--profile-name]  
                     
[--path -p]  
                     
[--storage-class -sc]
```
### <a name="examples"></a>示例
部署数据控制器。
```bash
azdata arc dc create
```
### <a name="required-parameters"></a>必需参数
#### `--namespace -ns`
数据控制器要部署到的 Kubernetes 命名空间。 如果此命名空间已存在，则将使用它。 如果此命名空间不存在，则将尝试先创建它。
#### `--name -n`
数据控制器的名称。
#### `--connectivity-mode`
与操作数据控制器的 Azure 建立间接或直接的连接。
#### `--resource-group -g`
应在其中添加数据控制器资源的 Azure 资源组。
#### `--location -l`
将在其中存储数据控制器元数据的 Azure 位置（例如美国东部）。
#### `--subscription -s`
应在其中添加数据控制器资源的 Azure 订阅 ID。
### <a name="optional-parameters"></a>可选参数
#### `--profile-name`
现有配置文件的名称。 有关可用选项，请运行 `azdata arc dc config list`。 以下源之一：['azure-arc-aks-premium-storage', 'azure-arc-ake', 'azure-arc-openshift', 'azure-arc-gke', 'azure-arc-aks-default-storage', 'azure-arc-kubeadm', 'azure-arc-eks', 'azure-arc-azure-openshift', 'azure-arc-aks-hci'].
#### `--path -p`
包含要使用的自定义配置文件的目录的路径。 若要创建自定义配置文件，请运行 `azdata arc dc config init`。
#### `--storage-class -sc`
用于所有数据的存储类，以及需要它们的所有数据控制器 pod 的日志永久性卷。
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
## <a name="azdata-arc-dc-delete"></a>azdata arc dc delete
删除数据控制器 - 系统上需要 kube 配置。
```bash
azdata arc dc delete --name -n 
                     --namespace -ns  
                     
[--force -f]
```
### <a name="examples"></a>示例
部署数据控制器。
```bash
azdata arc dc delete
```
### <a name="required-parameters"></a>必需参数
#### `--name -n`
数据控制器名称。
#### `--namespace -ns`
数据控制器所在的 Kubernetes 命名空间。
### <a name="optional-parameters"></a>可选参数
#### `--force -f`
强制删除数据控制器。
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
## <a name="azdata-arc-dc-export"></a>azdata arc dc export
将指标、日志或使用情况导出到文件。
```bash
azdata arc dc export --type -t 
                     --path -p  
                     
[--force -f]
```
### <a name="required-parameters"></a>必需参数
#### `--type -t`
要导出的数据的类型。 选项：日志、指标和使用情况。
#### `--path -p`
完整路径或相对路径，包括要导出的文件的文件名。
### <a name="optional-parameters"></a>可选参数
#### `--force -f`
强制创建输出文件。 覆盖同一路径中的任何现有文件。
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
## <a name="azdata-arc-dc-upload"></a>azdata arc dc upload
将从数据控制器导出的数据文件上传到 Azure。
```bash
azdata arc dc upload --path -p 
                     
```
### <a name="required-parameters"></a>必需参数
#### `--path -p`
完整路径或相对路径，包括要上传的文件的文件名。
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

