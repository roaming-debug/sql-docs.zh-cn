---
title: azdata arc postgres endpoint reference
titleSuffix: SQL Server big data clusters
description: azdata arc postgres endpoint 命令的参考文章。
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 09/22/2020
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: cb48072eaee6cdee6498655f05b88ae05c2062a8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100048997"
---
# <a name="azdata-arc-postgres-endpoint"></a>azdata arc postgres endpoint

适用于 [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

以下文章提供了 azdata 工具中 sql 命令的参考。 有关其他 azdata 命令的详细信息，请参阅 [azdata 参考](reference-azdata.md)

## <a name="commands"></a>命令

|命令|说明|
| --- | --- |
[azdata arc postgres endpoint list](#azdata-arc-postgres-endpoint-list) | 列出 PostgreSQL 服务器组终结点。
## <a name="azdata-arc-postgres-endpoint-list"></a>azdata arc postgres endpoint list
列出 PostgreSQL 服务器组终结点。
```bash
azdata arc postgres endpoint list --name -n 
                                  [--engine-version -ev]
```
### <a name="examples"></a>示例
列出 PostgreSQL 服务器组终结点。
```bash
azdata arc postgres endpoint list -n postgres01
```
### <a name="required-parameters"></a>必需参数
#### `--name -n`
PostgreSQL 服务器组的名称。
### <a name="optional-parameters"></a>可选参数
#### `--engine-version -ev`
当不同引擎版本的两个服务器组具有相同的名称时，可以将 --engine-version 与 --name 结合使用，以标识 PostgreSQL 超大规模服务器组。 --engine-version 是可选的，并且在用于标识服务器组时，它必须为 11 或 12。
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

