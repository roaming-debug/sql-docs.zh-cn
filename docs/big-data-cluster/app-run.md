---
title: 使用 azdata 运行应用程序
titleSuffix: SQL Server Big Data Clusters
description: 在 SQL Server 2019 大数据群集上使用 azdata 运行应用程序。
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.metadata: seo-lt-2019
ms.date: 08/16/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: b83c79398dbcc6c7d55eb0ddfe6cb1d8f0d398ed
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100039587"
---
# <a name="run-apps-with-azdata---sql-server-big-data-clusters"></a>使用 azdata 运行应用 - SQL Server 大数据群集

本文介绍如何在 SQL Server 大数据群集内运行应用程序。

## <a name="prerequisites"></a>先决条件

- [SQL Server 2019 大数据群集](deployment-guidance.md)
- [[!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]](../azdata/install/deploy-install-azdata.md)

## <a name="capabilities"></a>功能

在 SQL Server 2019 中，可以创建、删除、描述、初始化、列出运行和更新应用程序。 下表介绍了可以与 azdata 一起使用的应用程序部署命令。

|Command |描述 |
|:---|:---|
|`azdata app describe` | 描述应用程序。 |
|`azdata app run` | 运行应用程序。 |


下面各部分详细说明了这些参数。


## <a name="run-an-app"></a>运行应用

如果应用处于 `Ready` 状态，则可以使用指定的输入参数运行它。 使用以下语法运行应用：

```bash
azdata app run --name <app_name> --version <app_version> --inputs <inputs_params>
```

下面的示例命令演示了 run 命令：

```bash
azdata app run --name add-app --version v1 --inputs x=1,y=2
```

如果运行成功，应会看到创建应用时所指定的输出。 以下输出是一个示例。

```json
{
  "changedFiles": [],
  "consoleOutput": "",
  "errorMessage": "",
  "outputFiles": {},
  "outputParameters": {
    "result": 3
  },
  "success": true
}
```


## <a name="describe-an-app"></a>描述应用

describe 命令提供有关应用程序的详细信息，包括群集中的终结点。 应用开发人员通常使用此命令通过 swagger 客户端来生成应用，并使用 Web 服务以 RESTful 的方式与应用进行交互。 有关详细信息，请参阅[在大数据群集上使用应用程序](app-consume.md)。

```json
{
  "input_param_defs": [
    {
      "name": "x",
      "type": "int"
    },
    {
      "name": "y",
      "type": "int"
    }
  ],
  "links": {
    "app": "https://10.1.1.3:30080/api/app/add-app/v1",
    "swagger": "https://10.1.1.3:30080/api/app/add-app/v1/swagger.json"
  },
  "name": "add-app",
  "output_param_defs": [
    {
      "name": "result",
      "type": "int"
    }
  ],
  "state": "Ready",
  "version": "v1"
}
```

如需了解如何将部署在 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] 上的应用集成到自己的应用程序中，请参阅[使用大数据群集上的应用程序](app-consume.md)获取详细信息。 有关其他示例，请参阅[应用部署示例](https://aka.ms/sql-app-deploy)。

有关 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] 的详细信息，请参阅[什么是 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]？](big-data-cluster-overview.md)。