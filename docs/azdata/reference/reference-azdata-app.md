---
title: azdata app reference
titleSuffix: SQL Server big data clusters
description: azdata app 命令的参考文章。
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 09/22/2020
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: d2278059053366fb89e8ecd5a7cd626980bec637
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052708"
---
# <a name="azdata-app"></a>azdata app

适用于 [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

以下文章提供了 azdata 工具中 sql 命令的参考。 有关其他 azdata 命令的详细信息，请参阅 [azdata 参考](reference-azdata.md)

## <a name="commands"></a>命令

|命令|描述|
| --- | --- |
[azdata app template](reference-azdata-app-template.md) | 模板。
[azdata app init](#azdata-app-init) | 启动新应用程序主干。
[azdata app create](#azdata-app-create) | 创建应用程序。
[azdata app update](#azdata-app-update) | 更新应用程序。
[azdata app list](#azdata-app-list) | 列出应用程序。
[azdata app delete](#azdata-app-delete) | 删除应用程序。
[azdata app run](#azdata-app-run) | 运行应用程序。
[azdata app describe](#azdata-app-describe) | 描述应用程序。
## <a name="azdata-app-init"></a>azdata app init
帮助基于运行时环境启动新应用程序主干和/或规范文件。
```bash
azdata app init [--spec -s] 
                [--name -n]  
                
[--version -v]  
                
[--template -t]  
                
[--destination -d]  
                
[--url -u]
```
### <a name="examples"></a>示例
仅搭建新应用程序 `spec.yaml` 的基架。
```bash
azdata app init --spec
```
基于 `r` 模板搭建新 R 应用程序主干。
```bash
azdata app init --name reduce --template r
```
基于 `python` 模板搭建新 Python 应用程序主干。
```bash
azdata app init --name reduce --template python
```
基于 `ssis` 模板搭建新 SSIS 应用程序主干。
```bash
azdata app init --name reduce --template ssis            
```
### <a name="optional-parameters"></a>可选参数
#### `--spec -s`
仅生成一个应用程序 spec.yaml。
#### `--name -n`
应用程序名称。
#### `--version -v`
应用程序版本。
#### `--template -t`
模板名称。 有关支持的模板名称的完整列表，运行 `azdata app template list`
#### `--destination -d`
应用程序主干的放置位置。 默认：当前工作目录。
#### `--url -u`
指定其他模板存储库位置。 默认： https://github.com/Microsoft/SQLBDC-AppDeploy.git
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
## <a name="azdata-app-create"></a>azdata app create
创建应用程序。
```bash
azdata app create --spec -s 
                  
```
### <a name="examples"></a>示例
从包含有效 spec.yaml 部署规范的目录中创建新的应用程序。
```bash
azdata app create --spec /path/to/dir/with/spec/yaml
```
### <a name="required-parameters"></a>必需的参数
#### `--spec -s`
含有 YAML 规范文件的目录的路径，该文件用于描述应用程序。
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
## <a name="azdata-app-update"></a>azdata app update
更新应用程序。
```bash
azdata app update [--spec -s] 
                  [--yes -y]
```
### <a name="examples"></a>示例
从包含有效 spec.yaml 部署规范的目录更新现有应用程序。
```bash
azdata app update --spec /path/to/dir/with/spec/yaml    
```
### <a name="optional-parameters"></a>可选参数
#### `--spec -s`
含有 YAML 规范文件的目录的路径，该文件用于描述应用程序。
#### `--yes -y`
从 CWD 的 spec.yaml 文件更新应用程序时，不提示确认信息。
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
## <a name="azdata-app-list"></a>azdata app list
列出应用程序。
```bash
azdata app list [--name -n] 
                [--version -v]
```
### <a name="examples"></a>示例
按名称和版本列出应用程序。
```bash
azdata app list --name reduce  --version v1
```
按名称列出所有应用程序版本。
```bash
azdata app list --name reduce
```
按名称列出所有应用程序版本。
```bash
azdata app list
```
### <a name="optional-parameters"></a>可选参数
#### `--name -n`
应用程序名称。
#### `--version -v`
应用程序版本。
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
## <a name="azdata-app-delete"></a>azdata app delete
删除应用程序。
```bash
azdata app delete --name -n 
                  --version -v
```
### <a name="examples"></a>示例
按名称和版本删除应用程序。
```bash
azdata app delete --name reduce --version v1    
```
### <a name="required-parameters"></a>必需的参数
#### `--name -n`
应用程序名称。
#### `--version -v`
应用程序版本。
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
## <a name="azdata-app-run"></a>azdata app run
运行应用程序。
```bash
azdata app run --name -n 
               --version -v  
               
[--inputs]
```
### <a name="examples"></a>示例
运行无输入参数的应用程序。
```bash
azdata app run --name reduce --version v1
```
运行具有 1 个输入参数的应用程序。
```bash
azdata app run --name reduce --version v1 --inputs x=10
```
运行具有多个输入参数的应用程序。
```bash
azdata app run --name reduce --version v1 --inputs x=10,y5.6    
```
### <a name="required-parameters"></a>必需的参数
#### `--name -n`
应用程序名称。
#### `--version -v`
应用程序版本。
### <a name="optional-parameters"></a>可选参数
#### `--inputs`
CSV `name=value` 格式的应用程序输入参数。
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
## <a name="azdata-app-describe"></a>azdata app describe
描述应用程序。
```bash
azdata app describe [--spec -s] 
                    [--name -n]  
                    
[--version -v]
```
### <a name="examples"></a>示例
描述应用程序。
```bash
azdata app describe --name reduce --version v1    
```
### <a name="optional-parameters"></a>可选参数
#### `--spec -s`
含有 YAML 规范文件的目录的路径，该文件用于描述应用程序。
#### `--name -n`
应用程序名称。
#### `--version -v`
应用程序版本。
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

