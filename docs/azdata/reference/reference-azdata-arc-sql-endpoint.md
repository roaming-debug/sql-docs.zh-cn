---
title: azdata arc sql endpoint 参考
titleSuffix: SQL Server big data clusters
description: azdata arc sql endpoint 命令的参考文章。
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 190bc4360d8f8cd35f0ceda04887a1fc912243e3
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/13/2021
ms.locfileid: "100342536"
---
# <a name="azdata-arc-sql-endpoint"></a>azdata arc sql endpoint

适用于 [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

以下文章提供了 azdata 工具中 sql 命令的参考。 有关其他 azdata 命令的详细信息，请参阅 [azdata 参考](reference-azdata.md)

## <a name="commands"></a>命令
|命令|描述|
| --- | --- |
[azdata arc sql endpoint list](#azdata-arc-sql-endpoint-list) | 列出 SQL 终结点。
## <a name="azdata-arc-sql-endpoint-list"></a>azdata arc sql endpoint list
列出 SQL 终结点。
```bash
azdata arc sql endpoint list [--name -n] 
                             
```
### <a name="examples"></a>示例
列出 SQL 托管实例的终结点。
```bash
azdata arc sql endpoint list -n sqlmi1
```
### <a name="optional-parameters"></a>可选参数
#### `--name -n`
要显示的 SQL 实例的名称。 如果省略，则显示所有实例的所有终结点。
### <a name="global-arguments"></a>全局参数
#### `--debug`
提高日志记录详细程度以显示所有调试日志。
#### `--help -h`
显示此帮助消息并退出。
#### `--output -o`
输出格式。  允许的值：json、jsonc、table、tsv。  默认值：json。
#### `--query -q`
JMESPath 查询字符串。 请参阅 [http://jmespath.org/](http://jmespath.org/)，获取详细信息和示例。
#### `--verbose`
提高日志记录详细程度。 使用 --debug 获取完整的调试日志。

## <a name="next-steps"></a>后续步骤

有关其他 azdata 命令的详细信息，请参阅 [azdata 参考](reference-azdata.md)。 

有关如何安装 azdata 工具的详细信息，请参阅[安装 azdata](..\install\deploy-install-azdata.md)。
