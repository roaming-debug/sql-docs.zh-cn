---
title: azdata arc sql mi 参考
titleSuffix: SQL Server big data clusters
description: azdata arc sql mi 命令的参考文章。
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 09/22/2020
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 6a3d561f6b09ca6720a70a067e5985fbc96dceb1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100048947"
---
# <a name="azdata-arc-sql-mi"></a>azdata arc sql mi

适用于 [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

以下文章提供了 azdata 工具中 sql 命令的参考。 有关其他 azdata 命令的详细信息，请参阅 [azdata 参考](reference-azdata.md)

## <a name="commands"></a>命令

|命令|说明|
| --- | --- |
[azdata arc sql mi create](#azdata-arc-sql-mi-create) | 创建 SQL 托管实例。
[azdata arc sql mi edit](#azdata-arc-sql-mi-edit) | 编辑 SQL 托管实例的配置。
[azdata arc sql mi delete](#azdata-arc-sql-mi-delete) | 删除 SQL 托管实例。
[azdata arc sql mi show](#azdata-arc-sql-mi-show) | 显示 SQL 托管实例的详细信息。
[azdata arc sql mi list](#azdata-arc-sql-mi-list) | 列出 SQL 托管实例。
[azdata arc sql mi config](reference-azdata-arc-sql-mi-config.md) | 配置命令。
## <a name="azdata-arc-sql-mi-create"></a>azdata arc sql mi create
若要设置 SQL 托管实例的密码，请设置环境变量 AZDATA_PASSWORD
```bash
azdata arc sql mi create --name -n 
                         [--path]  
                         
[--cores-limit -cl]  
                         
[--cores-request -cr]  
                         
[--memory-limit -ml]  
                         
[--memory-request -mr]  
                         
[--storage-class-data -scd]  
                         
[--storage-class-logs -scl]  
                         
[--storage-class-data-logs -scdl]  
                         
[--storage-class-backups -scb]  
                         
[--volume-size-data -vsd]  
                         
[--volume-size-logs -vsl]  
                         
[--volume-size-data-logs -vsdl]  
                         
[--volume-size-backups -vsb]  
                         
[--no-external-endpoint]  
                         
[--dev]  
                         
[--no-wait]
```
### <a name="examples"></a>示例
创建 SQL 托管实例。
```bash
azdata arc sql mi create -n sqlmi1
```
### <a name="required-parameters"></a>必需参数
#### `--name -n`
SQL 托管实例的名称。
### <a name="optional-parameters"></a>可选参数
#### `--path`
指向 SQL 托管实例 json 文件的 src 文件的路径。
#### `--cores-limit -cl`
托管实例的核心限制（整数）。
#### `--cores-request -cr`
对托管实例核心（整数）的请求。
#### `--memory-limit -ml`
托管实例容量的限制（整数）。
#### `--memory-request -mr`
对托管实例容量（整数 GB 内存量）的请求。
#### `--storage-class-data -scd`
要用于数据的存储类 (.mdf)。 如果未指定任何值，则不会指定存储类，这将导致 Kubernetes 使用默认存储类。
#### `--storage-class-logs -scl`
要用于日志的存储类 (/var/log)。 如果未指定任何值，则不会指定存储类，这将导致 Kubernetes 使用默认存储类。
#### `--storage-class-data-logs -scdl`
要用于数据库日志的存储类 (.ldf)。 如果未指定任何值，则不会指定存储类，这将导致 Kubernetes 使用默认存储类。
#### `--storage-class-backups -scb`
要用于备份的存储类 (/var/opt/mssql/backups)。 如果未指定任何值，则不会指定存储类，这将导致 Kubernetes 使用默认存储类。
#### `--volume-size-data -vsd`
要用于数据的存储卷的大小（正数），后跟 Ki（千字节）、Mi（兆字节）或 Gi（吉字节）。
#### `--volume-size-logs -vsl`
要用于日志的存储卷的大小（正数），后跟 Ki（千字节）、Mi（兆字节）或 Gi（吉字节）。
#### `--volume-size-data-logs -vsdl`
要用于数据日志的存储卷的大小（正数），后跟 Ki（千字节）、Mi（兆字节）或 Gi（吉字节）。
#### `--volume-size-backups -vsb`
要用于备份的存储卷的大小（正数），后跟 Ki（千字节）、Mi（兆字节）或 Gi（吉字节）。
#### `--no-external-endpoint`
如果已指定，则不会创建外部服务。 否则，将使用与数据控制器相同的服务类型来创建外部服务。
#### `--dev`
如果指定了此项，则会将其视为开发实例，而不会对其进行计费。
#### `--no-wait`
如果给定，该命令不会等待实例处于就绪状态再返回。
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
## <a name="azdata-arc-sql-mi-edit"></a>azdata arc sql mi edit
编辑 SQL 托管实例的配置。
```bash
azdata arc sql mi edit --name -n 
                       [--path]  
                       
[--cores-limit -cl]  
                       
[--cores-request -cr]  
                       
[--memory-limit -ml]  
                       
[--memory-request -mr]  
                       
[--dev]  
                       
[--no-wait]
```
### <a name="examples"></a>示例
编辑 SQL 托管实例的配置。
```bash
azdata arc sql mi edit --path ./spec.json -n sqlmi1
```
### <a name="required-parameters"></a>必需参数
#### `--name -n`
正在编辑的 SQL 托管实例的名称。 不能更改部署实例时为其指定的名称。
### <a name="optional-parameters"></a>可选参数
#### `--path`
指向 SQL 托管实例 json 文件的 src 文件的路径。
#### `--cores-limit -cl`
托管实例的核心限制（整数）。
#### `--cores-request -cr`
对托管实例核心（整数）的请求。
#### `--memory-limit -ml`
托管实例容量的限制（整数）。
#### `--memory-request -mr`
对托管实例容量（整数 GB 内存量）的请求。
#### `--dev`
如果指定了此项，则会将其视为开发实例，而不会对其进行计费。
#### `--no-wait`
如果给定，该命令不会等待实例处于就绪状态再返回。
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
## <a name="azdata-arc-sql-mi-delete"></a>azdata arc sql mi delete
删除 SQL 托管实例。
```bash
azdata arc sql mi delete --name -n 
                         
```
### <a name="examples"></a>示例
删除 SQL 托管实例。
```bash
azdata arc sql mi delete -n sqlmi1
```
### <a name="required-parameters"></a>必需参数
#### `--name -n`
要删除的 SQL 托管实例的名称。
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
## <a name="azdata-arc-sql-mi-show"></a>azdata arc sql mi show
显示 SQL 托管实例的详细信息。
```bash
azdata arc sql mi show --name -n 
                       [--path -p]
```
### <a name="examples"></a>示例
显示 SQL 托管实例的详细信息。
```bash
azdata arc sql mi show -n sqlmi1
```
### <a name="required-parameters"></a>必需参数
#### `--name -n`
要显示的 SQL 托管实例的名称。
### <a name="optional-parameters"></a>可选参数
#### `--path -p`
应将 SQL 托管实例的完整规范写入到的路径。 如果省略，则会将规范写入标准输出。
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
## <a name="azdata-arc-sql-mi-list"></a>azdata arc sql mi list
列出 SQL 托管实例。
```bash
azdata arc sql mi list 
```
### <a name="examples"></a>示例
列出 SQL 托管实例。
```bash
azdata arc sql mi list
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

## <a name="next-steps"></a>后续步骤

有关其他 azdata 命令的详细信息，请参阅 [azdata 参考](reference-azdata.md)。 

有关如何安装 azdata 工具的详细信息，请参阅[安装 azdata](..\install\deploy-install-azdata.md)。

