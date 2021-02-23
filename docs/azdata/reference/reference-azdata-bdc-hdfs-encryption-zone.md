---
title: azdata bdc hdfs encryption-zone 参考
titleSuffix: SQL Server big data clusters
description: azdata bdc hdfs encryption-zone 命令的参考文章。
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 7c094d489cb925e280f5a44a8234d70fc817554b
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/13/2021
ms.locfileid: "100342520"
---
# <a name="azdata-bdc-hdfs-encryption-zone"></a>azdata bdc hdfs encryption-zone

适用于 [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

以下文章提供了 azdata 工具中 sql 命令的参考。 有关其他 azdata 命令的详细信息，请参阅 [azdata 参考](reference-azdata.md)

## <a name="commands"></a>命令
|命令|描述|
| --- | --- |
[azdata bdc hdfs encryption-zone create](#azdata-bdc-hdfs-encryption-zone-create) | 将 HDFS 文件夹转换为加密区域。
[azdata bdc hdfs encryption-zone list](#azdata-bdc-hdfs-encryption-zone-list) | 列出所有加密区域以及与加密区域相关的密钥。
[azdata bdc hdfs encryption-zone get-file-encryption-info](#azdata-bdc-hdfs-encryption-zone-get-file-encryption-info) | 获取给定 HDFS 文件路径的加密信息。
[azdata bdc hdfs encryption-zone status](#azdata-bdc-hdfs-encryption-zone-status) | 获取 HDFS 群集的重新加密状态。
[azdata bdc hdfs encryption-zone reencrypt](#azdata-bdc-hdfs-encryption-zone-reencrypt) | 控制--path 参数指定的加密区域的重新加密。
## <a name="azdata-bdc-hdfs-encryption-zone-create"></a>azdata bdc hdfs encryption-zone create
使用所提供的密钥将 HDFS 文件夹转换为加密区域，对加密区域中的文件进行加密。
```bash
azdata bdc hdfs encryption-zone create --path -p 
                                       --keyname -k
```
### <a name="examples"></a>示例
要将现有文件夹 /user/securefolder 转换为加密区域，请使用名为 securelake 的密钥
```bash
azdata bdc hdfs encryption-zone create --path /home/securefolder --keyname securelake
```
### <a name="required-parameters"></a>必需参数
#### `--path -p`
要转换为加密区域的 HDFS 文件夹的路径。
#### `--keyname -k`
用于保护加密区域的 Hadoop KMS 密钥。
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
## <a name="azdata-bdc-hdfs-encryption-zone-list"></a>azdata bdc hdfs encryption-zone list
列出所有加密区域以及与加密区域相关的密钥。
```bash
azdata bdc hdfs encryption-zone list 
```
### <a name="examples"></a>示例
列出所有加密区域。
```bash
azdata bdc hdfs encryption-zone list
```
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
## <a name="azdata-bdc-hdfs-encryption-zone-get-file-encryption-info"></a>azdata bdc hdfs encryption-zone get-file-encryption-info
获取给定 HDFS 文件路径的加密信息。
```bash
azdata bdc hdfs encryption-zone get-file-encryption-info --path -p 
                                                         
```
### <a name="examples"></a>示例
获取 /user/securefolder/data.csv 下的文件的加密信息。
```bash
azdata bdc hdfs encryption-zone get-file-encryption-info --path /user/securefolder/data.csv
```
### <a name="required-parameters"></a>必需参数
#### `--path -p`
应对其检索加密信息的 HDFS 文件路径。
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
## <a name="azdata-bdc-hdfs-encryption-zone-status"></a>azdata bdc hdfs encryption-zone status
获取 HDFS 群集的重新加密状态。
```bash
azdata bdc hdfs encryption-zone status 
```
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
## <a name="azdata-bdc-hdfs-encryption-zone-reencrypt"></a>azdata bdc hdfs encryption-zone reencrypt
控制--path 参数指定的加密区域的重新加密。
```bash
azdata bdc hdfs encryption-zone reencrypt --path -p 
                                          --action -a
```
### <a name="examples"></a>示例
开始对加密区域 securelake 进行重新加密。
```bash
azdata bdc hdfs encryption-zone reencrypt --path /securelake --action start
```
取消对加密区域 securelake 进行重新加密。
```bash
azdata bdc hdfs encryption-zone reencrypt --path /securelake --action cancel
```
### <a name="required-parameters"></a>必需参数
#### `--path -p`
应重新加密的 HDFS 加密区域文件夹的路径
#### `--action -a`
要执行的重新加密操作。 有效值为 start 和 cancel
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
