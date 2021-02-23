---
title: azdata bdc sql settings 参考
titleSuffix: SQL Server big data clusters
description: azdata bdc sql settings 命令的参考文章。
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 208f136f246cbcd6af612fbd4867a2a50c0f0cae
ms.sourcegitcommit: 129c084add904fd3f7e9ab35a800c3fd8b1a8927
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/17/2021
ms.locfileid: "100567287"
---
# <a name="azdata-bdc-sql-settings"></a>azdata bdc sql settings

适用于 [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

以下文章提供了 azdata 工具中 sql settings 命令的参考 。 有关其他 azdata 命令的详细信息，请参阅 [azdata 参考](reference-azdata.md)

## <a name="commands"></a>命令
|命令|描述|
| --- | --- |
[azdata bdc sql settings set](#azdata-bdc-sql-settings-set) | 设置 sql 服务范围设置。
[azdata bdc sql settings show](#azdata-bdc-sql-settings-show) | 显示指定资源的 sql 服务范围设置和（可选）sql 设置。

## <a name="azdata-bdc-sql-settings-set"></a>azdata bdc sql settings set
提供设置服务范围或资源范围设置的功能。 可指定设置的全名和值。 不会将设置应用于正在运行的 BDC。 若要应用到正在运行的 BDC，请运行升级。
```bash
azdata bdc sql settings set --settings -s 
                        
```
### <a name="examples"></a>示例
在 SQL Server 主实例中启用 SQL 代理。
```bash 
azdata bdc sql settings set --settings mssql.sqlagent.enabled=true --resources master 
``` 
在数据池中设置 SQL Server 的 CPU 数。
```bash 
azdata bdc sql settings set --settings mssql.numberOfCpus=10 –resources data-0 
``` 

### <a name="required-parameters"></a>必需参数
#### `--settings -s`
为所提供的设置设置已配置的值。 可使用逗号分隔列表设置多个设置。
### <a name="optional-parameters"></a>可选参数 
#### `--resources` 
为所提供的资源设置给定的设置。 可使用逗号分隔列表的形式列出资源。 

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

## <a name="azdata-bdc-sql-settings-show"></a>azdata bdc sql settings show
显示 BDC 的 `sql` 服务范围（资源范围 [可选]）设置。 默认情况下，此命令显示用户配置的服务范围设置。 筛选器可用于显示所有设置（系统管理的和可配置的设置）、可配置的设置或挂起的设置。 若要查看特定的服务范围或资源范围设置，可以提供设置名称。 使用“recursive”可显示服务中所有资源的设置。 
```bash

azdata bdc sql settings show 
[--settings -s]
[--filter-option -f]  
[--recursive -rec]
[--include-details -i]  
[--description -d]
```
### <a name="examples"></a>示例
显示用户配置的 SQL 服务范围设置 
```bash
azdata bdc sql settings show
```
显示数据池中的最大服务器内存 SQL 配置。
```bash
azdata bdc sql settings show --settings mssql.maxServerMemory --resources data-0 
```
显示 SQL 服务范围和资源范围设置的挂起设置更改。
```bash
azdata bdc sql settings show --filter-options=pending --recursive --include-details
```
### <a name="optional-parameters"></a>可选参数 
#### `--filter-options | -f` 
用于筛选显示哪种服务级别或资源范围设置而不是仅显示用户配置的设置的选项。 可以使用筛选器来显示所有设置（系统管理的和用户可配置的设置）、所有可配置的设置或挂起的设置。 选项：`userConfigured``all`、`pending`、`configurable`。
#### `--settings | -s` 
显示指定设置名称的信息 
#### `--include-details | -i` 
包括选择要显示的设置的其他详细信息 
#### `--description | -d` 
包括设置说明。 必须与 --include-details 一起使用 
#### `--resources | -r` 
显示给定资源的设置信息。 可使用逗号分隔列表的形式列出资源。 
#### `--recursive | -rec` 
显示给定范围（服务或服务资源范围）的设置信息，以及所有较低范围的组件（资源）设置信息。 

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

有关如何安装 azdata 工具的详细信息，请参阅[安装 azdata 以管理 SQL Server 2019 大数据群集](../install/deploy-install-azdata.md)。
