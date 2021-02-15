---
title: SQL Server dacpac 扩展
description: 了解如何安装和启动数据层应用程序向导，它可便于轻松部署和提取 dacpac 文件，以及导入和导出 bacpac 文件。
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: yualan
ms.author: alayu
ms.reviewer: maghan, sstein
ms.custom: ''
ms.date: 11/04/2019
ms.openlocfilehash: e0cfe057767488eb0a7ddb741934a10410898ffa
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100040257"
---
# <a name="sql-server-dacpac-extension"></a>SQL Server dacpac 扩展

“数据层应用程序向导”提供了一种易用的向导体验，可用于部署和提取 dacpac 文件以及导入和导出 bacpac 文件。

## <a name="features"></a>功能

* 将 dacpac 部署到 SQL Server 实例
* 将 SQL Server 实例提取到 dacpac
* 从 bacpac 创建数据库
* 将架构和数据导出到 bacpac

![dacpac 扩展演示 gif](media/sql-server-dacpac-extension/dacpac-extension-demo.gif)

## <a name="why-would-i-use-the-data-tier-application-wizard"></a>为什么要使用数据层应用程序向导？

通过该向导，可以更轻松地管理 dacpac 和 bacpac 文件，从而简化支持应用程序的数据层元素的开发和部署。 若要了解有关使用数据层应用程序的详细信息，[请查看我们的文档。](../../relational-databases/data-tier-applications/data-tier-applications.md)

## <a name="install-the-extension"></a>安装扩展

1. 选择“扩展”图标以查看可用扩展。

    ![扩展管理器图标](media/add-extensions/extension-manager-icon.png)

2. 搜索“SQL Server dacpac”扩展并选择它以查看其详细信息  。 选择“安装”以添加扩展。

3. 安装后，重载以启用 Azure Data Studio 中的扩展（仅在第一次安装扩展时需要进行此操作）  。

## <a name="launch-the-data-tier-application-wizard"></a>启动数据层应用程序向导

若要启动该向导，请右键单击“数据库”文件夹，或右键单击对象资源管理器中的特定数据库。 然后，选择“提取数据层应用程序向导”。

![dacpac 扩展启动菜单](media/sql-server-dacpac-extension/dacpac-extension-launch.png)

## <a name="next-steps"></a>后续步骤

若要了解有关 dacpac 的详细信息，[请查看我们的文档。](../../relational-databases/data-tier-applications/data-tier-applications.md)
请在[此处](https://github.com/microsoft/azuredatastudio/issues)报告问题和提出功能请求。