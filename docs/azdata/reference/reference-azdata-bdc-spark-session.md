---
title: azdata bdc spark session 参考
titleSuffix: SQL Server big data clusters
description: azdata bdc spark session 命令的参考文章。
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 09/22/2020
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 6d48576bf62b71c66a8bbdc33766084e2aaed37f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052408"
---
# <a name="azdata-bdc-spark-session"></a>azdata bdc spark session

适用于 [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

以下文章提供了 azdata 工具中 sql 命令的参考。 有关其他 azdata 命令的详细信息，请参阅 [azdata 参考](reference-azdata.md)

## <a name="commands"></a>命令

|命令|描述|
| --- | --- |
[azdata bdc spark session create](#azdata-bdc-spark-session-create) | 创建一个新的 Spark 会话。
[azdata bdc spark session list](#azdata-bdc-spark-session-list) | 列出 Spark 中的所有活动会话。
[azdata bdc spark session info](#azdata-bdc-spark-session-info) | 获取有关活动 Spark 会话的信息。
[azdata bdc spark session log](#azdata-bdc-spark-session-log) | 获取活动 Spark 会话的执行日志。
[azdata bdc spark session state](#azdata-bdc-spark-session-state) | 获取活动 Spark 会话的执行状态。
[azdata bdc spark session delete](#azdata-bdc-spark-session-delete) | 删除 Spark 会话。
## <a name="azdata-bdc-spark-session-create"></a>azdata bdc spark session create
这将创建一个新的交互式 Spark 会话。 调用方必须指定 Spark 会话的类型。 此会话超出了 azdata 执行的生存期，必须使用“spark session delete”删除它
```bash
azdata bdc spark session create [--session-kind -k] 
                                [--jar-files -j]  
                                
[--py-files -p]  
                                
[--files -f]  
                                
[--driver-memory]  
                                
[--driver-cores]  
                                
[--executor-memory]  
                                
[--executor-cores]  
                                
[--executor-count]  
                                
[--archives -a]  
                                
[--queue -q]  
                                
[--name -n]  
                                
[--config -c]  
                                
[--timeout-seconds -t]
```
### <a name="examples"></a>示例
创建会话。
```bash
azdata bdc spark session create --session-kind pyspark
```
### <a name="optional-parameters"></a>可选参数
#### `--session-kind -k`
要创建的会话类型的名称。  spark、pyspark、sparkr 或 sql 之一。
#### `--jar-files -j`
jar 文件路径列表。  若要传入列表，JSON 会对值进行编码。  示例：‘["entry1", "entry2"]’。
#### `--py-files -p`
python文件路径列表。  若要传入列表，JSON 会对值进行编码。  示例：‘["entry1", "entry2"]’。
#### `--files -f`
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
#### `--archives -a`
存档路径列表。  若要传入列表，JSON 会对值进行编码。  示例：‘["entry1", "entry2"]’。
#### `--queue -q`
要在其中执行会话的 Spark 队列的名称。
#### `--name -n`
Spark 会话的名称。
#### `--config -c`
包含 Spark 配置值的名称值对的列表。  编码为 JSON 字典。  示例：'{"name":"value", "name2":"value2"}'。
#### `--timeout-seconds -t`
会话空闲超时值（以秒为单位）。
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
## <a name="azdata-bdc-spark-session-list"></a>azdata bdc spark session list
列出 Spark 中的所有活动会话。
```bash
azdata bdc spark session list 
```
### <a name="examples"></a>示例
列出所有活动会话。
```bash
azdata bdc spark session list
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
## <a name="azdata-bdc-spark-session-info"></a>azdata bdc spark session info
这将获取具有给定 ID 的活动 Spark 会话的会话信息。  会话 ID 从“spark session create”返回。
```bash
azdata bdc spark session info --session-id -i 
                              
```
### <a name="examples"></a>示例
获取 ID 为 0 的会话的会话信息。
```bash
azdata bdc spark session info --session-id 0
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
## <a name="azdata-bdc-spark-session-log"></a>azdata bdc spark session log
这将获取具有给定 ID 的活动 Spark 会话的会话日志条目。  会话 ID 从“spark session create”返回。
```bash
azdata bdc spark session log --session-id -i 
                             
```
### <a name="examples"></a>示例
获取 ID 为 0 的会话的会话日志。
```bash
azdata bdc spark session log --session-id 0
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
## <a name="azdata-bdc-spark-session-state"></a>azdata bdc spark session state
这将获取具有给定 ID 的活动 Spark 会话的会话状态。  会话 ID 从“spark session create”返回。
```bash
azdata bdc spark session state --session-id -i 
                               
```
### <a name="examples"></a>示例
获取 ID 为 0 的会话的会话状态。
```bash
azdata bdc spark session state --session-id 0
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
## <a name="azdata-bdc-spark-session-delete"></a>azdata bdc spark session delete
这将删除交互式 Spark 会话。 会话 ID 从“spark session create”返回。
```bash
azdata bdc spark session delete --session-id -i 
                                
```
### <a name="examples"></a>示例
删除会话。
```bash
azdata bdc spark session delete --session-id 0
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

## <a name="next-steps"></a>后续步骤

有关其他 azdata 命令的详细信息，请参阅 [azdata 参考](reference-azdata.md)。 

有关如何安装 azdata 工具的详细信息，请参阅[安装 azdata](..\install\deploy-install-azdata.md)。

