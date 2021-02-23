---
title: azdata bdc settings 参考
titleSuffix: SQL Server big data clusters
description: azdata bdc settings 命令的参考文章。
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 37688a962fc17679a1a642af1b83609d2b8f480a
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/13/2021
ms.locfileid: "100342492"
---
# <a name="azdata-bdc-settings"></a>azdata bdc settings

适用于 [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

以下文章提供了 azdata 工具中 sql 命令的参考。 有关其他 azdata 命令的详细信息，请参阅 [azdata 参考](reference-azdata.md)

## <a name="commands"></a>命令
|命令|描述|
| --- | --- |
[azdata bdc settings set](#azdata-bdc-settings-set) | 设置群集范围设置。
[azdata bdc settings apply](#azdata-bdc-settings-apply) | 将挂起的设置更改应用于 BDC。
[azdata bdc settings cancel-apply](#azdata-bdc-settings-cancel-apply) | 取消应用 BDC 设置。
[azdata bdc settings show](#azdata-bdc-settings-show) | 使用 --recursive 显示群集范围设置或所有群集设置。
[azdata bdc settings revert](#azdata-bdc-settings-revert) | 在所有范围内还原 BDC 中挂起的设置更改。
## <a name="azdata-bdc-settings-set"></a>azdata bdc settings set
设置群集范围设置。 可指定设置的全名和值。 运行 apply 以应用设置并更新 BDC 设置。
```bash
azdata bdc settings set --settings -s 
                        
```
### <a name="examples"></a>示例
设置群集的“bdc.telemetry.customerFeedback”默认值。
```bash
azdata bdc settings set --settings bdc.telemetry.customerFeedback=false
```
### <a name="required-parameters"></a>必需参数
#### `--settings -s`
为所提供的设置设置已配置的值。 可使用逗号分隔列表设置多个设置。
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
## <a name="azdata-bdc-settings-apply"></a>azdata bdc settings apply
将挂起的设置更改应用于 BDC。
```bash
azdata bdc settings apply [--force -f] 
                          
```
### <a name="examples"></a>示例
将挂起的设置更改应用于 BDC。
```bash
azdata bdc settings apply
```
强制应用，系统不会提示用户进行任何确认且所有问题都将作为 stderr 的一部分输出。
```bash
azdata bdc settings apply --force
```
### <a name="optional-parameters"></a>可选参数
#### `--force -f`
强制应用，系统不会提示用户进行任何确认且所有问题都将作为 stderr 的一部分输出。
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
## <a name="azdata-bdc-settings-cancel-apply"></a>azdata bdc settings cancel-apply
如果在设置应用程序期间出现故障，则会将大数据群集返回其上次运行状态。 如果应用于正在运行的群集，则此命令将为 no-op。 取消后，之前挂起的设置更改仍将处于挂起状态。
```bash
azdata bdc settings cancel-apply [--force -f] 
                                 
```
### <a name="examples"></a>示例
取消应用 BDC 设置。
```bash
azdata bdc settings cancel-apply
```
强制取消应用，系统不会提示用户进行任何确认且所有问题都将作为 stderr 的一部分输出。
```bash
azdata bdc settings cancel-apply --force
```
### <a name="optional-parameters"></a>可选参数
#### `--force -f`
强制取消应用，系统不会提示用户进行任何确认且所有问题都将作为 stderr 的一部分输出。
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
## <a name="azdata-bdc-settings-show"></a>azdata bdc settings show
显示 BDC 的群集级别设置。 默认情况下，此命令显示用户配置的群集范围设置。 可以使用其他筛选器来显示所有设置（系统管理的以及用户可配置的和继承的设置）、所有可配置的设置或挂起的设置。 若要查看特定的群集范围设置，可以提供设置名称。 若要查看所有范围（群集、服务和资源）内的设置，可以指定“recursive”。
```bash
azdata bdc settings show [--settings -s] 
                         [--filter-option -f]  
                         
[--recursive -rec]  
                         
[--include-details -i]  
                         
[--description -d]
```
### <a name="examples"></a>示例
显示是否启用了 BDC 遥测数据收集。
```bash
azdata bdc settings show --settings bdc.telemetry.customerFeedback
```
在所有范围内显示 BDC 中用户配置的设置。
```bash
azdata bdc settings show --recursive
```
在所有范围内显示 BDC 中所有挂起的设置。
```bash
azdata bdc settings show –filter-option=pending --recursive
```
### <a name="optional-parameters"></a>可选参数
#### `--settings -s`
显示指定设置名称的信息。
#### `--filter-option -f`
筛选要显示哪些群集范围设置，而不是仅显示“用户配置的”设置。 可以使用筛选器来显示所有设置（系统管理的和用户可配置的设置）、所有可配置的设置或挂起的设置。
`userConfigured`
#### `--recursive -rec`
显示群集范围的设置信息以及所有较低范围的组件（服务、资源）设置信息。
#### `--include-details -i`
包括选择要显示的设置的其他详细信息。
#### `--description -d`
包括设置说明。
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
## <a name="azdata-bdc-settings-revert"></a>azdata bdc settings revert
在所有范围内还原 BDC 中挂起的设置更改。
```bash
azdata bdc settings revert [--force -f] 
                           
```
### <a name="examples"></a>示例
在所有范围内还原 BDC 挂起的设置更改。
```bash
azdata bdc settings revert
```
强制还原，系统不会提示用户进行任何确认且所有问题都将作为 stderr 的一部分输出。
```bash
azdata bdc settings revert --force
```
### <a name="optional-parameters"></a>可选参数
#### `--force -f`
强制还原，系统不会提示用户进行任何确认且所有问题都将作为 stderr 的一部分输出。
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
