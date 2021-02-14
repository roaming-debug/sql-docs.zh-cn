---
title: 连接到数据库并浏览现有对象
description: 了解如何在 Visual Studio 中使用 SQLServer 对象资源管理器同时连接到本地和非本地 SQL Server 实例。
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
f1_keywords:
- sql.data.tools.connectionpicker.f1
ms.assetid: 9b331800-3806-4459-ac58-88cdc98124d3
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: b2a8c7ae0bbd50598d6c476c0a065786e6efcddb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018417"
---
# <a name="how-to-connect-to-a-database-and-browse-existing-objects"></a>如何：连接到数据库并浏览现有对象

数据库管理员和开发人员的一个非常常见的任务是连接到活动数据库，并且针对其对象设计或浏览其架构和查询。 Visual Studio 中的 SQL Server 对象资源管理器现在包含一个专用的 SQL Server 节点，在该节点下，所有连接的 SQL Server 实例及其数据库都在类似 SSMS 的层次结构下进行分组。 连接的 SQL Server 实例可以是内部实例，例如运行 SQL Server 2008 的实例，也可以是外部 SQL Azure 实例。  
  
下面的过程假定您已安装了 AdventureWorks 示例数据库。 可以使用 [GitHub](https://github.com/Microsoft/sql-server-samples/releases/tag/adventureworks) 为不同的 SQL Server 版本查找并安装示例数据库。 如果您愿意，还可以执行以下步骤并使用您的服务器上的现有数据库。  
  
### <a name="to-connect-to-a-database-instance"></a>连接到一个数据库实例  
  
1.  在 Visual Studio 中，请确保 SQL Server 对象资源管理器处于打开状态。 如果未打开，请单击“视图”菜单并且选择“SQL Server 对象资源管理器”。  
  
2.  右键单击“SQL Server 对象资源管理器”中的“SQL Server”节点，然后选择“添加 SQL Server”。  
  
3.  在 **“连接到服务器”** 对话框中，输入您要连接到的服务器实例的 **“服务器名称”** 和您的凭据，然后单击 **“连接”**。  
  
4.  在“SQL Server 对象资源管理器”中，展开你的服务器实例下的“数据库”节点。 您将看到在此 **“数据库”** 节点下添加的此服务器实例中驻留的所有数据库。  
  
5.  展开 AdventureWorks 节点（或其他数据库节点）。 你将注意到，所有数据库实体都组织到类似 SQL Server Management Studio 的层次结构中。  
  
