---
title: azdata bdc spark batch 参考
titleSuffix: SQL Server big data clusters
description: azdata bdc spark batch 命令的参考文章。
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 09/22/2020
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 8e6dea113ff65125f24819b1f579f686e9452ee1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100048797"
---
# <a name="azdata-bdc-spark-batch"></a>azdata bdc spark batch

适用于 [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

以下文章提供了 azdata 工具中 sql 命令的参考。 有关其他 azdata 命令的详细信息，请参阅 [azdata 参考](reference-azdata.md)

## <a name="commands"></a>命令

|命令|描述|
| --- | --- |
[azdata bdc spark batch create](#azdata-bdc-spark-batch-create) | 创建新的 Spark 批处理。
[azdata bdc spark batch list](#azdata-bdc-spark-batch-list) | 列出 Spark 中的所有批处理。
[azdata bdc spark batch info](#azdata-bdc-spark-batch-info) | 获取有关活动 Spark 批处理的信息。
[azdata bdc spark batch log](#azdata-bdc-spark-batch-log) | 获取 Spark 批处理的执行日志。
[azdata bdc spark batch state](#azdata-bdc-spark-batch-state) | 获取 Spark 批处理的执行状态。
[azdata bdc spark batch delete](#azdata-bdc-spark-batch-delete) | 删除 Spark 批处理。
## <a name="azdata-bdc-spark-batch-create"></a>azdata bdc spark batch create
此命令创建一个新的批处理 Spark 作业，以执行提供的代码。
```bash
azdata bdc spark batch create --file -f 
                              [--class-name -c]  
                              
[--arguments -a]  
                              
[--jar-files -j]  
                              
[--py-files -p]  
                              
[--files]  
                              
[--driver-memory]  
                              
[--driver-cores]  
                              
[--executor-memory]  
                              
[--executor-cores]  
                              
[--executor-count]  
                              
[--archives]  
                              
[--queue -q]  
                              
[--name -n]  
                              
[--config]
```
### <a name="examples"></a>示例
创建新的 Spark 批处理。
```bash
azdata bdc spark batch create --code "2+2"
```
### <a name="required-parameters"></a>必需的参数
#### `--file -f`
要执行的文件的路径。
### <a name="optional-parameters"></a>可选参数
#### `--class-name -c`
传入一个或多个 jar 文件时要执行的类的名称。
#### `--arguments -a`
参数列表。  若要传入列表，JSON 会对值进行编码。  示例：‘["entry1", "entry2"]’。
#### `--jar-files -j`
jar 文件路径列表。  若要传入列表，JSON 会对值进行编码。  示例：‘["entry1", "entry2"]’。
#### `--py-files -p`
python文件路径列表。  若要传入列表，JSON 会对值进行编码。  示例：‘["entry1", "entry2"]’。
#### `--files`
文件路径列表。  若要传入列表，JSON 会对值进行编码。  示例：‘["entry1", "entry2"]’。
#### `--driver-memory`
要分配给驱动程序的内存量。  将单位指定为值的一部分。  例如 512M 或 2G。
#### `--driver-cores`
要分配给驱动程序的 CPU 内核数。
#### `--executor-memory`
要分配给执行程序的内存量。  将单位指定为值的一部分。  例如 512M 或 2G。
#### `--executor-cores`
要分配给执行程序的 CPU 内核数。
#### `--executor-count`
要运行的执行程序的实例数。
#### `--archives`
存档路径列表。  若要传入列表，JSON 会对值进行编码。  示例：‘["entry1", "entry2"]’。
#### `--queue -q`
要在其中执行会话的 Spark 队列的名称。
#### `--name -n`
Spark 会话的名称。
#### `--config`
包含 Spark 配置值的名称值对的列表。  编码为 JSON 字典。  示例：'{"name":"value", "name2":"value2"}'。
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
## <a name="azdata-bdc-spark-batch-list"></a>azdata bdc spark batch list
列出 Spark 中的所有批处理。
```bash
azdata bdc spark batch list 
```
### <a name="examples"></a>示例
列出所有活动的批处理。
```bash
azdata bdc spark batch list
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
## <a name="azdata-bdc-spark-batch-info"></a>azdata bdc spark batch info
此命令可获取具有给定 ID 的 Spark 批处理的信息。  批处理 ID 从“spark batch create”返回。
```bash
azdata bdc spark batch info --batch-id -i 
                            
```
### <a name="examples"></a>示例
获取 ID 为 0 的批处理的信息。
```bash
azdata bdc spark batch info --batch-id 0
```
### <a name="required-parameters"></a>必需的参数
#### `--batch-id -i`
Spark 批处理 ID 号。
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
## <a name="azdata-bdc-spark-batch-log"></a>azdata bdc spark batch log
此命令可获取具有给定 ID 的 Spark 批处理的批处理日志条目。  批处理 ID 从“spark batch create”返回。
```bash
azdata bdc spark batch log --batch-id -i 
                           
```
### <a name="examples"></a>示例
获取 ID 为 0 的批处理的日志。
```bash
azdata bdc spark batch log --batch-id 0
```
### <a name="required-parameters"></a>必需的参数
#### `--batch-id -i`
Spark 批处理 ID 号。
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
## <a name="azdata-bdc-spark-batch-state"></a>azdata bdc spark batch state
此命令可获取具有给定 ID 的 Spark 批处理的批处理状态。  批处理 ID 从“spark batch create”返回。
```bash
azdata bdc spark batch state --batch-id -i 
                             
```
### <a name="examples"></a>示例
获取 ID 为 0 的批处理的状态。
```bash
azdata bdc spark batch state --batch-id 0
```
### <a name="required-parameters"></a>必需的参数
#### `--batch-id -i`
Spark 批处理 ID 号。
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
## <a name="azdata-bdc-spark-batch-delete"></a>azdata bdc spark batch delete
此命令可删除 Spark 批处理。 批处理 ID 从“spark batch create”返回。
```bash
azdata bdc spark batch delete --batch-id -i 
                              
```
### <a name="examples"></a>示例
删除批处理。
```bash
azdata bdc spark batch delete --batch-id 0
```
### <a name="required-parameters"></a>必需的参数
#### `--batch-id -i`
Spark 批处理 ID 号。
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

