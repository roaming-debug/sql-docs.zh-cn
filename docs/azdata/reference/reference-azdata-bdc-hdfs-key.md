---
title: azdata bdc hdfs key 参考
titleSuffix: SQL Server big data clusters
description: azdata bdc hdfs key 命令的参考文章。
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: c208d1c726bae41b349abdf475627afbc82ed2b8
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/13/2021
ms.locfileid: "100342500"
---
# <a name="azdata-bdc-hdfs-key"></a>azdata bdc hdfs key

适用于 [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

以下文章提供了 azdata 工具中 sql 命令的参考。 有关其他 azdata 命令的详细信息，请参阅 [azdata 参考](reference-azdata.md)

## <a name="commands"></a>命令
|命令|描述|
| --- | --- |
[azdata bdc hdfs key create](#azdata-bdc-hdfs-key-create) | 创建 HDFS 密钥。
[azdata bdc hdfs key list](#azdata-bdc-hdfs-key-list) | 列出所有 Hadoop 加密区域密钥。
[azdata bdc hdfs key roll](#azdata-bdc-hdfs-key-roll) | 滚动 HDFS 密钥。
## <a name="azdata-bdc-hdfs-key-create"></a>azdata bdc hdfs key create
使用给定的名称和给定的大小创建 HDFS 密钥。
```bash
azdata bdc hdfs key create --name -n 
                           [--size -size]
```
### <a name="examples"></a>示例
要创建名称为 key1 的 256 位密钥，azdata hdfs key create --name key1 --size 256
```bash
azdata hdfs key create --name key1 --size 256
```
### <a name="required-parameters"></a>必需参数
#### `--name -n`
Hadoop 加密区域密钥的名称。 
### <a name="optional-parameters"></a>可选参数
#### `--size -size`
Hadoop 加密密钥的位长度，默认长度为 256。
`256`
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
## <a name="azdata-bdc-hdfs-key-list"></a>azdata bdc hdfs key list
列出所有 Hadoop 加密区域密钥。
```bash
azdata bdc hdfs key list 
```
### <a name="examples"></a>示例
列出所有 Hadoop 加密区域密钥，azdata bdc hdfs key list
```bash
azdata bdc hdfs key list
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
## <a name="azdata-bdc-hdfs-key-roll"></a>azdata bdc hdfs key roll
滚动具有给定名称的 HDFS 密钥。
```bash
azdata bdc hdfs key roll --name -n 
                         
```
### <a name="examples"></a>示例
若要滚动名称为 key1 的密钥，azdata hdfs key roll --name key1。
```bash
azdata hdfs key roll --name key1
```
### <a name="required-parameters"></a>必需参数
#### `--name -n`
要滚动到新版本的加密区域密钥的名称。 
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
