---
title: azdata arc resource-kind reference
titleSuffix: SQL Server big data clusters
description: azdata arc resource-kind 命令的参考文章。
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 09/22/2020
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: b13ab0788b0cef102fa5388492ef8ad12836c138
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052598"
---
# <a name="azdata-arc-resource-kind"></a>azdata arc resource-kind

适用于 [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

以下文章提供了 azdata 工具中 sql 命令的参考。 有关其他 azdata 命令的详细信息，请参阅 [azdata 参考](reference-azdata.md)

## <a name="commands"></a>命令

|命令|说明|
| --- | --- |
[azdata arc resource-kind list](#azdata-arc-resource-kind-list) | 列出可定义和创建的 Arc 可用自定义资源类型。
[azdata arc resource-kind get](#azdata-arc-resource-kind-get) | 获取 Arc 资源类型的模板文件。
## <a name="azdata-arc-resource-kind-list"></a>azdata arc resource-kind list
列出可定义和创建的 Arc 可用自定义资源类型。 列出后，可继续获取定义或创建该自定义资源时所需的模板文件。
```bash
azdata arc resource-kind list 
```
### <a name="examples"></a>示例
用于列出 Arc 的可用自定义资源类型的示例命令。
```bash
azdata arc resource-kind list
```
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
## <a name="azdata-arc-resource-kind-get"></a>azdata arc resource-kind get
获取 Arc 资源类型的模板文件。
```bash
azdata arc resource-kind get --kind -k 
                             [--dest -d]
```
### <a name="examples"></a>示例
用于获取 Arc 资源类型的 CRD 模板文件的示例命令。
```bash
azdata arc resource-kind get --kind sqldb
```
### <a name="required-parameters"></a>必需参数
#### `--kind -k`
模板文件所需的 Arc 资源的类型。
### <a name="optional-parameters"></a>可选参数
#### `--dest -d`
在其中放置模板文件的目录。
`template`
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

