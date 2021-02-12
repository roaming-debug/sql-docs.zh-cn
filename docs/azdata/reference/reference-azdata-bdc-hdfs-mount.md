---
title: azdata bdc hdfs mount reference
titleSuffix: SQL Server big data clusters
description: azdata bdc hdfs 装载命令的参考文章。
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 09/22/2020
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 539d5b331315d9e700ad7692a5840778ead87f0c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052428"
---
# <a name="azdata-bdc-hdfs-mount"></a>azdata bdc hdfs mount

适用于 [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

以下文章提供了 azdata 工具中 sql 命令的参考。 有关其他 azdata 命令的详细信息，请参阅 [azdata 参考](reference-azdata.md)

## <a name="commands"></a>命令

|命令|描述|
| --- | --- |
[azdata bdc hdfs mount create](#azdata-bdc-hdfs-mount-create) | 在 HDFS 中创建远程存储的装载。
[azdata bdc hdfs mount delete](#azdata-bdc-hdfs-mount-delete) | 在 HDFS 中删除远程存储的装载。
[azdata bdc hdfs mount status](#azdata-bdc-hdfs-mount-status) | 装载状态。
[azdata bdc hdfs mount refresh](#azdata-bdc-hdfs-mount-refresh) | 在 HDFS 中刷新装载的内容。
## <a name="azdata-bdc-hdfs-mount-create"></a>azdata bdc hdfs mount create
在 HDFS 中创建远程存储的装载。 应使用环境变量 MOUNT_CREDENTIALS 将用于访问远程存储的凭据（如果有）指定为逗号分隔的“键=值”对列表。 必须对键或值中的任何逗号进行转义。
```bash
azdata bdc hdfs mount create --remote-uri -r 
                             --mount-path -m
```
### <a name="examples"></a>示例
使用共享密钥在 HDFS 路径 /mounts/adlsv2/data 上的 ADLS Gen 2 帐户“adlsv2example”中装载容器“数据”
```bash
Set the MOUNT_CREDENTIALS environment variable as "fs.azure.abfs.account.name=<your-storage-account-name>.dfs.core.windows.net,fs.azure.account.key.<your-storage-account-name>.dfs.core.windows.net=<storage-account-access-key>".
azdata bdc hdfs mount create --remote-uri abfs://data@adlsv2example.dfs.core.windows.net/
    --mount-path /mounts/adlsv2/data
```
在本地 HDFS 路径 /mounts/hdfs/ 上装载远程 HDFS BDC (hdfs://namenode1:8080/)
```bash
azdata bdc hdfs mount create --remote-uri hdfs://namenode1:8080/ --mount-path /mounts/hdfs/
```
### <a name="required-parameters"></a>必需的参数
#### `--remote-uri -r`
要装载的远程存储的 URI（装载的源）。
#### `--mount-path -m`
要在其中创建装载的 HDFS 路径（装载目标）。
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
## <a name="azdata-bdc-hdfs-mount-delete"></a>azdata bdc hdfs mount delete
在 HDFS 中删除远程存储的装载。
```bash
azdata bdc hdfs mount delete --mount-path -m 
                             
```
### <a name="examples"></a>示例
删除在 /mounts/adlsv2/data 处为 ADLS Gen 2 存储帐户创建的装载。
```bash
azdata bdc hdfs mount delete --mount-path /mounts/adlsv2/data
```
### <a name="required-parameters"></a>必需的参数
#### `--mount-path -m`
与要删除的装载相对应的 HDFS 路径。
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
## <a name="azdata-bdc-hdfs-mount-status"></a>azdata bdc hdfs mount status
装载状态。
```bash
azdata bdc hdfs mount status [--mount-path -m] 
                             
```
### <a name="examples"></a>示例
按路径获取装载状态
```bash
azdata bdc hdfs mount status --mount-path /mounts/hdfs
```
获取所有装载的状态。
```bash
azdata bdc hdfs mount status
```
### <a name="optional-parameters"></a>可选参数
#### `--mount-path -m`
装载路径。
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
## <a name="azdata-bdc-hdfs-mount-refresh"></a>azdata bdc hdfs mount refresh
在 HDFS 中刷新装载的内容。
```bash
azdata bdc hdfs mount refresh --mount-path -m 
                              
```
### <a name="examples"></a>示例
刷新在 /mounts/adlsv2/data 处创建的装载。
```bash
azdata bdc hdfs mount refresh --mount-path /mounts/adlsv2/data
```
### <a name="required-parameters"></a>必需的参数
#### `--mount-path -m`
与要刷新的装载相对应的 HDFS 路径。
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

