---
title: azdata arc postgres server backup reference
titleSuffix: SQL Server big data clusters
description: azdata arc postgres server backup 命令的参考文章。
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 09/22/2020
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 00519fa62e0813bc25cee6599cd2faca9ed1489d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052648"
---
# <a name="azdata-arc-postgres-server-backup"></a>azdata arc postgres server backup

适用于 [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

以下文章提供了 azdata 工具中 sql 命令的参考。 有关其他 azdata 命令的详细信息，请参阅 [azdata 参考](reference-azdata.md)

## <a name="commands"></a>命令

|命令|说明|
| --- | --- |
[azdata arc postgres server backup create](#azdata-arc-postgres-server-backup-create) | 创建 PostgreSQL 服务器组的备份。
[azdata arc postgres server backup delete](#azdata-arc-postgres-server-backup-delete) | 删除 PostgreSQL 服务器组的备份。
[azdata arc postgres server backup restore](#azdata-arc-postgres-server-backup-restore) | 还原 PostgreSQL 服务器组的备份。
[azdata arc postgres server backup restorestatus](#azdata-arc-postgres-server-backup-restorestatus) | 获取最近的还原操作的状态（如果有）。
[azdata arc postgres server backup show](#azdata-arc-postgres-server-backup-show) | 显示 PostgreSQL 服务器组的备份详细信息。
## <a name="azdata-arc-postgres-server-backup-create"></a>azdata arc postgres server backup create
创建 PostgreSQL 服务器组的备份。
```bash
azdata arc postgres server backup create 
```
### <a name="examples"></a>示例
创建“pg”服务的备份。
```bash
azdata arc postgres server backup create -sn pg
```
创建“pg”服务的指定备份。
```bash
azdata arc postgres server backup create -sn pg -n backup1
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
## <a name="azdata-arc-postgres-server-backup-delete"></a>azdata arc postgres server backup delete
删除 PostgreSQL 服务器组的备份。
```bash
azdata arc postgres server backup delete 
```
### <a name="examples"></a>示例
删除 PostgreSQL 服务器组的备份。
```bash
azdata arc postgres backup delete -sn pg -id e07dd3940e374bd9acbc86869cbc123d
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
## <a name="azdata-arc-postgres-server-backup-restore"></a>azdata arc postgres server backup restore
还原 PostgreSQL 服务器组的备份。
```bash
azdata arc postgres server backup restore 
```
### <a name="examples"></a>示例
还原最新备份。
```bash
azdata arc postgres server restore -sn pg```
Restore a backup by ID
```bash
azdata arc postgres server restore -sn pg -id 123e4567e89b12d3a456426655440000
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
## <a name="azdata-arc-postgres-server-backup-restorestatus"></a>azdata arc postgres server backup restorestatus
获取最近的还原操作的状态（如果有）。
```bash
azdata arc postgres server backup restorestatus 
```
### <a name="examples"></a>示例
按 ID 获取“pg”服务的最近还原状态。
```bash
azdata arc postgres server backup restorestatus -sn pg -id 123e4567e89b12d3a456426655440000
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
## <a name="azdata-arc-postgres-server-backup-show"></a>azdata arc postgres server backup show
显示 PostgreSQL 服务器组的备份详细信息。
```bash
azdata arc postgres server backup show 
```
### <a name="examples"></a>示例
按 ID 获取“pg”服务的备份。
```bash
azdata arc postgres server backup show -sn pg -id 123e4567e89b12d3a456426655440000
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

