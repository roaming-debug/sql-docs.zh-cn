---
title: azdata postgres 参考
titleSuffix: SQL Server big data clusters
description: azdata postgres 命令的参考文章。
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 09/22/2020
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 6403db5f9b716684c3a791ade183a8f9c5edad47
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052268"
---
# <a name="azdata-postgres"></a>azdata postgres

适用于 [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

以下文章提供了 azdata 工具中 sql 命令的参考。 有关其他 azdata 命令的详细信息，请参阅 [azdata 参考](reference-azdata.md)

## <a name="commands"></a>命令

|命令|说明|
| --- | --- |
[azdata postgres shell](#azdata-postgres-shell) | 用于 Postgres 的命令行 shell 接口。 请参见https://www.pgcli.com/
[azdata postgres query](#azdata-postgres-query) | 通过 query 命令，可在数据库会话中执行 PostgreSQL 命令。
## <a name="azdata-postgres-shell"></a>azdata postgres shell
用于 Postgres 的命令行 shell 接口。 请参见https://www.pgcli.com/
```bash
azdata postgres shell [--dbname -d] 
                      [--host]  
                      
[--port -p]  
                      
[--password -w]  
                      
[--no-password]  
                      
[--single-connection]  
                      
[--username -u]  
                      
[--pgclirc]  
                      
[--dsn]  
                      
[--list-dsn]  
                      
[--row-limit]  
                      
[--less-chatty]  
                      
[--prompt]  
                      
[--prompt-dsn]  
                      
[--list -l]  
                      
[--auto-vertical-output]  
                      
[--warn]  
                      
[--no-warn]
```
### <a name="examples"></a>示例
用于启动交互式体验的示例命令行。
```bash
azdata postgres shell
```
使用提供的数据库和用户的示例命令行
```bash
azdata postgres shell --dbname <database> --username <username> --host <host>
```
用于开始使用完整连接字符串的示例命令行。
```bash
azdata postgres shell --dbname postgres://user:passw0rd@example.com:5432/master 
```
### <a name="optional-parameters"></a>可选参数
#### `--dbname -d`
要连接到的数据库名称。
#### `--host`
postgres 数据库的主机地址。
#### `--port -p`
postgres 实例侦听的端口号。
#### `--password -w`
强制密码提示。
#### `--no-password`
从不提示输入密码。
#### `--single-connection`
不要为完成使用单独的连接。
#### `--username -u`
用于连接到 postgres 数据库的用户名。
#### `--pgclirc`
pgclirc 文件的位置。
#### `--dsn`
使用在 pgclirc 文件的 [alias_dsn] 部分配置的 DSN。
#### `--list-dsn`
在 pgclirc 文件的 [alias_dsn] 部分配置的 DSN 的列表。
#### `--row-limit`
为行限制提示设置阈值。 使用 0 可禁止提示。
#### `--less-chatty`
启动时跳过简介，退出时再见。
#### `--prompt`
提示格式（默认值：“\u@\h:\d>”）。
#### `--prompt-dsn`
使用 DSN 假名的连接的提示格式（默认值：“\u@\h:\d>”）。
#### `--list -l`
列出可用数据库，然后退出。
#### `--auto-vertical-output`
如果结果大于终端宽度，则自动切换到垂直输出模式。
#### `--warn`
在运行破坏性查询之前发出警告。
#### `--no-warn`
在运行破坏性查询之前发出警告。
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
## <a name="azdata-postgres-query"></a>azdata postgres query
通过 query 命令，可在数据库会话中执行 PostgreSQL 命令。
```bash
azdata postgres query --q -q 
                      [--host]  
                      
[--dbname -d]  
                      
[--port -p]  
                      
[--username -u]
```
### <a name="examples"></a>示例
列出 information_schema 中的所有表。
```bash
azdata postgres query --host <host> --username <username> -q "SELECT * FROM information_schema.tables"
```
### <a name="required-parameters"></a>必需参数
#### `--q -q`
要执行的 PostgreSQL 查询。
### <a name="optional-parameters"></a>可选参数
#### `--host`
postgres 数据库的主机地址。
`localhost`
#### `--dbname -d`
要在其中运行查询的数据库。
#### `--port -p`
postgres 实例侦听的端口号。
`5432`
#### `--username -u`
用于连接到 postgres 数据库的用户名。
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

