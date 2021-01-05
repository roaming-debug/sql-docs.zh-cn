---
title: 通过扩展性添加其他功能
description: 了解用于扩展 Azure Data Studio 功能的扩展性模型和关键扩展性区域
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.reviewer: alayu, maghan, sstein
ms.custom: seodec18
ms.date: 09/24/2018
ms.openlocfilehash: 051e3afcd5f76624a1978c5ef3ef0f6a264fe07d
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/17/2020
ms.locfileid: "97637727"
---
# <a name="azure-data-studio-extensibility"></a>Azure Data Studio 扩展性

Azure Data Studio 具有多种扩展性机制，可以自定义用户体验并使这些自定义项可供整个用户社区使用。 核心 Azure Data Studio 平台基于 Visual Studio Code 构建，因此大多数 Visual Studio Code 扩展性 API 都可用。 此外，我们还为特定于数据管理的活动提供了额外的扩展点。

下面是一些关键扩展点：

- Visual Studio Code 扩展性 API
- Azure Data Studio 扩展创作工具
- 管理仪表板选项卡面板贡献
- 包含操作体验的见解
- Azure Data Studio 扩展性 API
- 自定义数据提供程序 API

## <a name="visual-studio-code-extensibility-apis"></a>Visual Studio Code 扩展性 API

由于核心 Azure Data Studio 平台是基于 Visual Studio Code 构建的，因此，Visual Studio Code 网站上的[扩展创作](https://code.visualstudio.com/docs/extensions/overview)和[扩展 API](https://code.visualstudio.com/docs/extensionAPI/overview) 文档中提供了有关 Visual Studio Code 扩展性 API 的详细信息。

> [!NOTE]
>  Azure Data Studio 版本与较新的 VS Code 版本一致，但其中包含的 VS Code 引擎可能不是当前的 VS Code 版本。 例如，在 2020 年 11月，Azure Data Studio 中的 VS Code 引擎为 1.48，而当前的 VS Code 版本为 1.51。  安装扩展时出现错误消息“无法安装扩展‘<name>’，因为它与 VS Code <version> 不兼容”，这是因为该扩展的程序包清单 (`package.json`) 中定义的 VS Code 引擎版本较新。 你可以通过“关于”下的“帮助”菜单验证 Azure Data Studio中的 VS Code 引擎版本。

## <a name="manage-dashboard-tab-panel-contributions"></a>管理仪表板选项卡面板贡献

有关详细信息，请参阅[贡献点](#contribution-points)和[上下文变量](#context-variables)。

## <a name="azure-data-studio-extensibility-apis"></a>Azure Data Studio 扩展性 API

有关详细信息，请参阅[扩展性 API](extensibility-apis.md)。


## <a name="contribution-points"></a>贡献点

本部分介绍 package.json 扩展清单中定义的各种贡献点。

azuredatastudio 中支持 IntelliSense。

### <a name="dashboard-contribution-points"></a>仪表板贡献点

向仪表板贡献选项卡、容器和/或见解小组件。

![仪表板](media/extensibility/dashboard-page.png)

`dashboard.tabs`

Dashboard.tabs 用于在仪表板页面内创建选项卡部分。 它需要对象或对象数组。  

```json
"dashboard.tabs": [
{
    "id": "test-tab1",
    "title": "Test 1",
    "description": "The test 1 displays a list of widgets.",
    "when": "connectionProvider == 'MSSQL' && !mssql:iscloud",
    "alwaysShow": true,
    "container": {
        ...
    }
}
]
```

`dashboard.containers`

可以使用 dashboard.containers 注册容器， 而不是内联指定仪表板容器（在 dashboard.tab 内）。 它接受对象或对象数组。

```json
"dashboard.containers": [
{
    "id": "innerTab1",
    "container": {
        ...
    }
},
{
    "id": "innerTab2",
    "container": {
       ...
    }
}
]
```

若要引用已注册的容器，请指定该容器的 ID

```json
"dashboard.tabs": [
{
    ...
    "container": {
        "innerTab1": {             
        }
    }
}
]

```

`dashboard.insights`

可以使用 dashboard.insights 注册见解。 这类似于[教程：生成自定义见解小组件](./tutorial-build-custom-insight-sql-server.md)

```json
"dashboard.insights": {
"id": "my-widget"
"type": {
    "count": {
        "dataDirection": "vertical",
        "dataType": "number",
        "legendPosition": "none",
           "labelFirstColumn": false,
        "columnsAsLabels": false
       }
   },
   "queryFile": "{your file folder}/activeSession.sql"
}
```


### <a name="dashboard-container-types"></a>仪表板容器类型

目前有四种受支持的容器类型：

1. `widgets-container`

    ![小组件容器](media/extensibility/widgets-container.png)

    将在容器中显示的小组件的列表。 它采用流布局。 它接受小组件列表。

    ```json
    "container": {
        "widgets-container": [
        {
            "widget": {
                "query-data-store-db-insight": {
                }
            }
        },
        {
            "widget": {
                "explorer-widget": {
                }
            }
        }
      ]
    }
    ```

2. `webview-container`

    ![webview 容器](media/extensibility/webview-container.png)

    webview 将显示在整个容器中。 它要求 webview ID 与选项卡 ID 相同

    ```json
    "container": {
        "webview-container": null
    }
    ```

3. `grid-container`

   ![网格容器](media/extensibility/grid-container.png)

   将在网格布局中显示的小组件或 webview 的列表

    ```json
    "container": {
        "grid-container": [
        {
            "name": "widget 1",
            "widget": {
                "explorer-widget": {
                }
            },
            "row":0,
            "col":0
        },
        {
            "name": "widget 2",
            "widget": {
                "tasks-widget": {
                    "backup", 
                    "restore",
                    "configureDashboard",
                    "newQuery"
                }
            },
            "row":0,
            "col":1
        },
        {
            "name": "Webview 1",
            "webview": {
                "id": "google"
            },
            "row":1,
            "col":0,
            "colspan":2
        },
        {
            "name": "widget 3",
            "widget": {
                "explorer-widget": {}
            },
            "row":0,
            "col":3,
            "rowspan":2
        },
    ]
    ```

4.  `nav-section`

    ![导航部分](media/extensibility/nav-section.png)

    导航部分将显示在容器中

    ```json
    "container": {
        "nav-section": [
            {
                "id": "innerTab1",
                "title": "inner-tab1",
                "icon": {
                    "light": "./icons/tab1Icon.svg",
                    "dark": "./icons/tab1Icon_dark.svg"
                },
                "container": {
                    ...
                }
            },
            {
                "id": "innerTab2",
                "title": "inner-tab2",
                "icon": {
                    "light": "./icons/tab2Icon.svg",
                    "dark": "./icons/tab2Icon_dark.svg"
                },
                "container": {
                    ...
                }
            }
        ]
    }
    ```



## <a name="context-variables"></a>上下文变量

有关 Visual Studio Code 中的上下文以及随后的 Azure Data Studio 的常规信息，请参阅[扩展性](https://code.visualstudio.com/docs/extensionAPI/extension-points#_example)。

在 Azure Data Studio 中，我们有关于数据库连接的特定上下文，可用于扩展。

### <a name="dashboard"></a>仪表板

在仪表板中，我们提供以下上下文变量：

|上下文变量| description|
|:---|:---|
|`connectionProvider` | 当前连接的提供程序的标识符字符串。 例如： `connectionProvider == 'MSSQL'`.|
|`serverName`|当前连接的服务器名称字符串。 例如： `serverName == 'localhost'`.|
|`databaseName` | 当前连接的数据库名称字符串。 例如： `databaseName == 'master'`.|
|`connection` | 当前连接的完整连接配置文件对象 (IConnectionProfile)|
|`dashboardContext` | 当前打开的仪表板页面的上下文字符串。 “database”或“server”。 例如： `dashboardContext == 'database'`|