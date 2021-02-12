---
title: azdata sql 参考
titleSuffix: SQL Server big data clusters
description: azdata sql 命令的参考文章。
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 09/22/2020
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: a7d142ca6afb77539b1425caaf097aca3959baeb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100048697"
---
# <a name="azdata-sql"></a>azdata sql

适用于 [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

以下文章提供了 azdata 工具中 sql 命令的参考。 有关其他 azdata 命令的详细信息，请参阅 [azdata 参考](reference-azdata.md)

## <a name="commands"></a>命令

|命令|描述|
| --- | --- |
[azdata sql shell](#azdata-sql-shell) | SQL CLI 允许用户通过 T-SQL 与 SQL Server 和 Azure SQL 交互。
[azdata sql query](#azdata-sql-query) | SQL CLI 允许用户通过 T-SQL 与 SQL Server 和 Azure SQL 交互。
## <a name="azdata-sql-shell"></a>azdata sql shell
SQL CLI 允许用户通过 T-SQL 与 SQL Server 和 Azure SQL 交互。
```bash
azdata sql shell [--username -u] 
                 [--database -d]  
                 
[--server -s]  
                 
[--integrated -e]  
                 
[--mssqlclirc]  
                 
[--row-limit]  
                 
[--less-chatty]  
                 
[--auto-vertical-output]  
                 
[--encrypt -n]  
                 
[--trust-server-certificate -c]  
                 
[--connect-timeout -l]  
                 
[--application-intent -k]  
                 
[--multi-subnet-failover -m]  
                 
[--packet-size]  
                 
[--dac-connection -a]  
                 
[--input-file -i]  
                 
[--output-file]  
                 
[--enable-sqltoolsservice-logging]  
                 
[--prompt]
```
### <a name="examples"></a>示例
用于启动交互式体验的示例命令行。
```bash
azdata sql shell
```
使用提供的服务器、用户和数据库的示例命令行
```bash
azdata sql shell --server localhost --username sa --database master         
```
### <a name="optional-parameters"></a>可选参数
#### `--username -u`
用于连接到数据库的用户名。
#### `--database -d`
要连接到的数据库名称。
#### `--server -s`
SQL Server 实例名称或地址。
#### `--integrated -e`
使用 Windows 上的集成身份验证。
#### `--mssqlclirc`
mssqlclirc 配置文件的位置。
#### `--row-limit`
设置行限制提示的阈值。 使用 0 可禁止提示。
#### `--less-chatty`
启动时跳过简介，退出时跳过再见。
#### `--auto-vertical-output`
如果结果大于终端宽度，则自动切换到垂直输出模式。
#### `--encrypt -n`
如果服务器安装了证书，则 SQL Server 对所有数据使用 SSL 加密。
#### `--trust-server-certificate -c`
在跳过用于验证信任的证书链遍历时将加密通道。
#### `--connect-timeout -l`
在终止请求之前等待连接到服务器的时间（以秒为单位）。
#### `--application-intent -k`
连接到 SQL Server 可用性组中的数据库时，声明应用程序工作负载类型。
#### `--multi-subnet-failover -m`
如果应用程序连接到不同子网上的 AlwaysOn 可用性组，则设置此选项可以更快地检测和连接到当前处于活动状态的服务器。
#### `--packet-size`
用于与 SQL Server 通信的网络数据包的大小（以字节为单位）。
#### `--dac-connection -a`
使用专用管理员连接连接到 SQL Server。
#### `--input-file -i`
指定包含一批 SQL 语句的文件以进行处理。
#### `--output-file`
指定从查询接收输出的文件。
#### `--enable-sqltoolsservice-logging`
启用 SqlToolsService 的诊断日志记录功能。
#### `--prompt`
提示格式（默认值：\d>
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
## <a name="azdata-sql-query"></a>azdata sql query
SQL CLI 允许用户通过 T-SQL 与 SQL Server 和 Azure SQL 交互。
```bash
azdata sql query -q 
                 [--database -d]  
                 
[--username -u]  
                 
[--server -s]  
                 
[--integrated -e]
```
### <a name="examples"></a>示例
用于选择表名称列表的示例命令行。
```bash
azdata sql query --server localhost --username sa --database master -q "SELECT name FROM SYS.TABLES"
```
### <a name="required-parameters"></a>必需参数
#### `-q`
要执行的 T-SQL 查询。
### <a name="optional-parameters"></a>可选参数
#### `--database -d`
要连接到的数据库名称。
`master`
#### `--username -u`
用于连接到数据库的用户名。
#### `--server -s`
SQL Server 实例名称或地址。
#### `--integrated -e`
使用 Windows 上的集成身份验证。
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

