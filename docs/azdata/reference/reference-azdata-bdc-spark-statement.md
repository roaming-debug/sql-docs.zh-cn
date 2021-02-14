---
title: azdata bdc spark statement 参考
titleSuffix: SQL Server big data clusters
description: azdata bdc spark statement 命令的参考文章。
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 09/22/2020
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 9f911c8d9e2a7f39d02e84c03d772a0390a388c7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100048787"
---
# <a name="azdata-bdc-spark-statement"></a>azdata bdc spark statement

适用于 [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

以下文章提供了 azdata 工具中 sql 命令的参考。 有关其他 azdata 命令的详细信息，请参阅 [azdata 参考](reference-azdata.md)

## <a name="commands"></a>命令

|命令|描述|
| --- | --- |
[azdata bdc spark statement list](#azdata-bdc-spark-statement-list) | 列出给定 Spark 会话中的所有语句。
[azdata bdc spark statement create](#azdata-bdc-spark-statement-create) | 在给定的会话中创建新的 Spark 语句。
[azdata bdc spark statement info](#azdata-bdc-spark-statement-info) | 获取给定 Spark 会话中有关请求的语句信息。
[azdata bdc spark statement cancel](#azdata-bdc-spark-statement-cancel) | 取消给定 Spark 会话内的语句。
## <a name="azdata-bdc-spark-statement-list"></a>azdata bdc spark statement list
列出给定 Spark 会话中的所有语句。
```bash
azdata bdc spark statement list --session-id -i 
                                
```
### <a name="examples"></a>示例
列出所有会话语句。
```bash
azdata bdc spark statement list --session-id 0
```
### <a name="required-parameters"></a>必需的参数
#### `--session-id -i`
Spark 会话 ID 号。
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
## <a name="azdata-bdc-spark-statement-create"></a>azdata bdc spark statement create
这会在给定会话中创建并执行新语句。  如果执行速度很快，则结果包含执行的输出。  此外，在语句完成后，可以使用“spark session info”检索结果。
```bash
azdata bdc spark statement create --session-id -i 
                                  --code -c
```
### <a name="examples"></a>示例
运行语句。
```bash
azdata bdc spark statement create --session-id 0 --code "2+2"
```
### <a name="required-parameters"></a>必需的参数
#### `--session-id -i`
Spark 会话 ID 号。
#### `--code -c`
包含要作为语句一部分执行的代码的字符串。
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
## <a name="azdata-bdc-spark-statement-info"></a>azdata bdc spark statement info
如果语句已完成，则将获取执行状态和执行结果。 语句 ID 从“spark statement create”返回。
```bash
azdata bdc spark statement info --session-id -i 
                                --statement-id -s
```
### <a name="examples"></a>示例
获取 ID 为 0 且语句 ID 为 0 的会话的语句信息。
```bash
azdata bdc spark statement info --session-id 0 --statement-id 0
```
### <a name="required-parameters"></a>必需的参数
#### `--session-id -i`
Spark 会话 ID 号。
#### `--statement-id -s`
给定会话 ID 内的 Spark 语句 ID 号。
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
## <a name="azdata-bdc-spark-statement-cancel"></a>azdata bdc spark statement cancel
这会取消给定 Spark 会话内的语句。 语句 ID 从“spark statement create”返回。
```bash
azdata bdc spark statement cancel --session-id -i 
                                  --statement-id -s
```
### <a name="examples"></a>示例
取消语句。
```bash
azdata bdc spark statement cancel --session-id 0 --statement-id 0
```
### <a name="required-parameters"></a>必需的参数
#### `--session-id -i`
Spark 会话 ID 号。
#### `--statement-id -s`
给定会话 ID 内的 Spark 语句 ID 号。
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

