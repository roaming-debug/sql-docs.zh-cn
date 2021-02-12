---
title: azdata bdc control status 参考
titleSuffix: SQL Server big data clusters
description: azdata bdc control status 命令的参考文章。
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 09/22/2020
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: eeb3b46c16fb332bcf2a506fda6481e407d0f989
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052488"
---
# <a name="azdata-bdc-control-status"></a>azdata bdc control status

适用于 [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

以下文章提供了 azdata 工具中 sql 命令的参考。 有关其他 azdata 命令的详细信息，请参阅 [azdata 参考](reference-azdata.md)

## <a name="commands"></a>命令

|命令|描述|
| --- | --- |
[azdata bdc control status show](#azdata-bdc-control-status-show) | 控制服务状态。
## <a name="azdata-bdc-control-status-show"></a>azdata bdc control status show
控制服务状态。
```bash
azdata bdc control status show [--resource -r] 
                               [--all -a]
```
### <a name="examples"></a>示例
获取服务的状态。
```bash
azdata bdc control status show
```
获取所有实例的控制服务状态。
```bash
azdata bdc control status show --all
```
获取控制服务中的控制资源的状态。
```bash
azdata bdc control status show --resource control
```
### <a name="optional-parameters"></a>可选参数
#### `--resource -r`
获取此服务中的这个资源。
#### `--all -a`
显示服务中每个资源的所有实例。
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

