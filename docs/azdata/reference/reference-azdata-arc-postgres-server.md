---
title: azdata arc postgres server reference
titleSuffix: SQL Server big data clusters
description: azdata arc postgres server 命令的参考文章。
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 09/22/2020
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 79d2cad23c6aed747837635e4d19d42a5daf41dd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052638"
---
# <a name="azdata-arc-postgres-server"></a>azdata arc postgres server

适用于 [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

以下文章提供了 azdata 工具中 sql 命令的参考。 有关其他 azdata 命令的详细信息，请参阅 [azdata 参考](reference-azdata.md)

## <a name="commands"></a>命令

|命令|说明|
| --- | --- |
[azdata arc postgres server create](#azdata-arc-postgres-server-create) | 创建 PostgreSQL 服务器组。
[azdata arc postgres server edit](#azdata-arc-postgres-server-edit) | 编辑 PostgreSQL 服务器组的配置。
[azdata arc postgres server delete](#azdata-arc-postgres-server-delete) | 创建 PostgreSQL 服务器组。
[azdata arc postgres server show](#azdata-arc-postgres-server-show) | 显示 PostgreSQL 服务器组的详细信息。
[azdata arc postgres server list](#azdata-arc-postgres-server-list) | 列出 PostgreSQL 服务器组。
[azdata arc postgres server config](reference-azdata-arc-postgres-server-config.md) | 配置命令。
[azdata arc postgres server backup](reference-azdata-arc-postgres-server-backup.md) | 管理 PostgreSQL 服务器组备份。
## <a name="azdata-arc-postgres-server-create"></a>azdata arc postgres server create
若要设置服务器组的密码，请设置环境变量 AZDATA_PASSWORD
```bash
azdata arc postgres server create --name -n 
                                  [--path]  
                                  
[--cores-limit -cl]  
                                  
[--cores-request -cr]  
                                  
[--memory-limit -ml]  
                                  
[--memory-request -mr]  
                                  
[--storage-class-data -scd]  
                                  
[--storage-class-logs -scl]  
                                  
[--storage-class-backups -scb]  
                                  
[--extensions]  
                                  
[--volume-size-data -vsd]  
                                  
[--volume-size-logs -vsl]  
                                  
[--volume-size-backups -vsb]  
                                  
[--workers -w]  
                                  
[--engine-version -ev]  
                                  
[--no-external-endpoint]  
                                  
[--dev]  
                                  
[--port]  
                                  
[--no-wait]  
                                  
[--engine-settings -e]
```
### <a name="examples"></a>示例
创建 PostgreSQL 服务器组。
```bash
azdata arc postgres server create -n pg1
```
使用引擎设置创建 PostgreSQL 服务器组。 下面的两个示例都有效。
```bash
azdata arc postgres server create -n pg1 --engine-settings "key1=val1"
azdata arc postgres server create -n pg1 --engine-settings "key2=val2"
```
### <a name="required-parameters"></a>必需参数
#### `--name -n`
实例的名称。
### <a name="optional-parameters"></a>可选参数
#### `--path`
PostgreSQL 服务器组的源 json 文件的路径。 该地址为可选。
#### `--cores-limit -cl`
每个节点可使用的 postgres 实例的最大 CPU 核心数。 支持小数核心数。
#### `--cores-request -cr`
每个节点计划服务时可使用的最小 CPU 核心数。 支持小数核心数。
#### `--memory-limit -ml`
Postgres 实例的内存限制，以数字表示，后跟 Ki（千字节）、Mi（兆字节）或 Gi（吉字节）。
#### `--memory-request -mr`
Postgres 实例的内存请求，以数字表示，后跟 Ki（千字节）、Mi（兆字节）或 Gi（吉字节）。
#### `--storage-class-data -scd`
要用于数据永久性卷的存储类。
#### `--storage-class-logs -scl`
要用于日志永久性卷的存储类。
#### `--storage-class-backups -scb`
要用于备份永久性卷的存储类。
#### `--extensions`
应在启动时加载的 Postgres 扩展的逗号分隔列表。 请参阅 postgres 文档了解支持的值。
#### `--volume-size-data -vsd`
要用于数据的存储卷的大小（正数），后跟 Ki（千字节）、Mi（兆字节）或 Gi（吉字节）。
#### `--volume-size-logs -vsl`
要用于日志的存储卷的大小（正数），后跟 Ki（千字节）、Mi（兆字节）或 Gi（吉字节）。
#### `--volume-size-backups -vsb`
要用于备份的存储卷的大小（正数），后跟 Ki（千字节）、Mi（兆字节）或 Gi（吉字节）。
#### `--workers -w`
要在分片群集中预配的工作器节点数，或为零（默认值），表示单节点 Postgres。
#### `--engine-version -ev`
必须为 11 或 12。 默认值为 12。
#### `--no-external-endpoint`
如果已指定，则不会创建外部服务。 否则，将使用与数据控制器相同的服务类型来创建外部服务。
#### `--dev`
如果指定了此项，则会将其视为开发实例，而不会对其进行计费。
#### `--port`
可选。
#### `--no-wait`
如果给定，该命令不会等待实例处于就绪状态再返回。
#### `--engine-settings -e`
以逗号分隔的 Postgres 引擎设置列表，格式为“key1=val1, key2=val2”。
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
## <a name="azdata-arc-postgres-server-edit"></a>azdata arc postgres server edit
编辑 PostgreSQL 服务器组的配置。
```bash
azdata arc postgres server edit --name -n 
                                [--path]  
                                
[--workers -w]  
                                
[--engine-version -ev]  
                                
[--cores-limit -cl]  
                                
[--cores-request -cr]  
                                
[--memory-limit -ml]  
                                
[--memory-request -mr]  
                                
[--extensions]  
                                
[--dev]  
                                
[--port]  
                                
[--no-wait]  
                                
[--engine-settings -e]  
                                
[--replace-engine-settings -re]  
                                
[--admin-password]
```
### <a name="examples"></a>示例
编辑 PostgreSQL 服务器组的配置。
```bash
azdata arc postgres server edit --src ./spec.json -n pg1
```
使用引擎设置编辑 PostgreSQL 服务器组。
```bash
azdata arc postgres server edit -n pg1 --engine-settings "key2=val2"
```
编辑 PostgreSQL 服务器组，并使用新设置 key1=val1 替换现有引擎设置。
```bash
azdata arc postgres server edit -n pg1 --engine-settings "key1=val1" --replace-engine-settings
```
### <a name="required-parameters"></a>必需参数
#### `--name -n`
正在编辑的 PostgreSQL 服务器组的名称。 不能更改部署实例时为其指定的名称。
### <a name="optional-parameters"></a>可选参数
#### `--path`
PostgreSQL 服务器组的源 json 文件的路径。 该地址为可选。
#### `--workers -w`
要在分片群集中预配的工作器节点数，或为零（默认值），表示单节点 Postgres。
#### `--engine-version -ev`
无法更改引擎版本。 当不同引擎版本的两个服务器组具有相同的名称时，可以将 --engine-version 与 --name 结合使用，以标识 PostgreSQL 超大规模服务器组。 --engine-version 是可选的，并且在用于标识服务器组时，它必须为 11 或 12。
#### `--cores-limit -cl`
每个节点可使用的 postgres 实例的最大 CPU 核心数，支持小数核心数。 若要删除 cores_limit，请将其值指定为空字符串。
#### `--cores-request -cr`
每个节点计划服务时可使用的最小 CPU 核心数，支持小数核心数。 若要删除 cores_request，请将其值指定为空字符串。
#### `--memory-limit -ml`
Postgres 实例的内存限制，以数字表示，后跟 Ki（千字节）、Mi（兆字节）或 Gi（吉字节）。 若要删除 memory_limit，请将其值指定为空字符串。
#### `--memory-request -mr`
Postgres 实例的内存请求，以数字表示，后跟 Ki（千字节）、Mi（兆字节）或 Gi（吉字节）。 若要删除 memory_request，请将其值指定为空字符串。
#### `--extensions`
应在启动时加载的 Postgres 扩展的逗号分隔列表。 请参阅 postgres 文档了解支持的值。
#### `--dev`
如果指定了此项，则会将其视为开发实例，而不会对其进行计费。
#### `--port`
可选。
#### `--no-wait`
如果给定，该命令不会等待实例处于就绪状态再返回。
#### `--engine-settings -e`
以逗号分隔的 Postgres 引擎设置列表，格式为“key1=val1, key2=val2”。 提供的设置将与现有设置合并。 若要删除某个设置，请提供一个空值，如“removedKey=”。 如果更改了需要重启的引擎设置，则将重启服务以立即应用该设置。
#### `--replace-engine-settings -re`
指定为 --engine-settings 后，会将所有现有自定义引擎设置替换为一组新的设置和值。
#### `--admin-password`
如果指定，Postgres 服务器的管理员密码将设置为 AZDATA_PASSWORD 环境变量的值（如果存在），否则为提示的值。
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
## <a name="azdata-arc-postgres-server-delete"></a>azdata arc postgres server delete
创建 PostgreSQL 服务器组。
```bash
azdata arc postgres server delete --name -n 
                                  [--engine-version -ev]
```
### <a name="examples"></a>示例
创建 PostgreSQL 服务器组。
```bash
azdata arc postgres server delete -n pg1
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
## <a name="azdata-arc-postgres-server-show"></a>azdata arc postgres server show
显示 PostgreSQL 服务器组的详细信息。
```bash
azdata arc postgres server show --name -n 
                                [--engine-version -ev]  
                                
[--path -p]
```
### <a name="examples"></a>示例
显示 PostgreSQL 服务器组的详细信息。
```bash
azdata arc postgres server show -n pg1
```
### <a name="required-parameters"></a>必需参数
#### `--name -n`
PostgreSQL 服务器组的名称。
### <a name="optional-parameters"></a>可选参数
#### `--engine-version -ev`
当不同引擎版本的两个服务器组具有相同的名称时，可以将 --engine-version 与 --name 结合使用，以标识 PostgreSQL 超大规模服务器组。 --engine-version 是可选的，并且在用于标识服务器组时，它必须为 11 或 12。
#### `--path -p`
应将 PostgreSQL 服务器组的完整规范写入到的路径。 如果省略，则会将规范写入标准输出。
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
## <a name="azdata-arc-postgres-server-list"></a>azdata arc postgres server list
列出 PostgreSQL 服务器组。
```bash
azdata arc postgres server list 
```
### <a name="examples"></a>示例
列出 PostgreSQL 服务器组。
```bash
azdata arc postgres server list
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

