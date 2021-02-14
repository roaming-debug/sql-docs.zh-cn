---
title: azdata bdc debug reference
titleSuffix: SQL Server big data clusters
description: Azdata bdc debug 命令的参考文章。
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 09/22/2020
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 68efee6c692b375c98a5dabf9dc69fa58aba21a5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052478"
---
# <a name="azdata-bdc-debug"></a>azdata bdc debug

适用于 [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

以下文章提供了 azdata 工具中 sql 命令的参考。 有关其他 azdata 命令的详细信息，请参阅 [azdata 参考](reference-azdata.md)

## <a name="commands"></a>命令

|命令|描述|
| --- | --- |
[azdata bdc debug copy-logs](#azdata-bdc-debug-copy-logs) | 复制日志。
[azdata bdc debug dump](#azdata-bdc-debug-dump) | 触发内存转储。
## <a name="azdata-bdc-debug-copy-logs"></a>azdata bdc debug copy-logs
从大数据群集复制调试日志 - 系统中需要 Kubernetes 配置。
```bash
azdata bdc debug copy-logs --namespace -ns 
                           [--container -c]  
                           
[--target-folder -d]  
                           
[--pod -p]  
                           
[--timeout -t]  
                           
[--skip-compress -sc]  
                           
[--exclude-dumps -ed]
```
### <a name="required-parameters"></a>必需的参数
#### `--namespace -ns`
大数据群集名称，用于 kubernetes 命名空间。
### <a name="optional-parameters"></a>可选参数
#### `--container -c`
复制具有相似名称的容器的日志（可选），默认情况下会复制所有容器的日志。 不能多次指定。 如果多次指定，则使用最后一个
#### `--target-folder -d`
要将日志复制到的目标文件夹路径。 （可选）默认情况下在本地文件夹中创建结果。  不能多次指定。 如果多次指定，则使用最后一个
#### `--pod -p`
复制具有相似名称的 Pod 的日志。 （可选）默认情况下会复制所有 Pod 的日志。 不能多次指定。 如果多次指定，则使用最后一个
#### `--timeout -t`
等待命令完成的秒数。 默认值为 0，表示无限制
#### `--skip-compress -sc`
是否跳过对结果文件夹的压缩。 默认值为 False，即压缩结果文件夹。
#### `--exclude-dumps -ed`
是否从结果文件夹中排除转储。 默认值为 False，即包含转储。
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
## <a name="azdata-bdc-debug-dump"></a>azdata bdc debug dump
触发内存转储，并从容器中复制 - 系统中需要 Kubernetes 配置。
```bash
azdata bdc debug dump --namespace -ns 
                      [--container -c]  
                      
[--target-folder -d]
```
### <a name="required-parameters"></a>必需的参数
#### `--namespace -ns`
大数据群集名称，用于 kubernetes 命名空间。
### <a name="optional-parameters"></a>可选参数
#### `--container -c`
为了转储正在运行的进程而要触发的目标容器 `controller`
#### `--target-folder -d`
要将转储复制出来的目标文件夹。`./output/dump`
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

