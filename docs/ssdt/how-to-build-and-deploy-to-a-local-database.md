---
title: 生成和部署到本地数据库
description: 了解 SQL server 2012 提供的本地服务器实例。 请参阅如何使用此实例生成、测试和调试开发项目。
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: ebca8ff8-9a09-4207-8979-9d577af7c1d5
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 598c75f5ef502d1679bd8ed7f17cb44c0f536abc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100063532"
---
# <a name="how-to-build-and-deploy-to-a-local-database"></a>如何：生成和部署到本地数据库

Microsoft SQL Server 2012 提供称作 SQL Server Express 本地数据库运行时的本地按需运行的服务器实例，在调试 SQL Server 数据库项目时该实例将被激活。 此本地服务器实例可以用作生成、测试和调试您的项目的沙盒。 它独立于已安装的任何 SQL Server 实例，并且不可从 SQL Server Data Tools (SSDT) 之外访问。 如果开发人员对生产数据库只有有限访问权限或没有访问权限，但想要在授权人士将其项目部署到生产前在本地测试其项目，则上述安排适合于此类开发人员。 此外，在您为 SQL Azure 开发数据库解决方案时，可以利用此本地服务器所提供的便利，在将数据库项目部署到云中之前在本地开发和测试您的数据库项目。  
  
> [!WARNING]  
> 在 SQL Server 对象资源管理器中的本地数据库节点下的数据库表示其相应的数据库项目，与连接的服务器实例中的同名数据库无关。  
  
> [!WARNING]  
> 以下过程利用在[连接的数据库开发](../ssdt/connected-database-development.md)和[面向项目的脱机数据库开发](../ssdt/project-oriented-offline-database-development.md)部分中的之前过程中创建的实体。  
  
### <a name="to-use-the-local-database"></a>使用本地数据库  
  
1.  请注意，在“SQL Server 对象资源管理器”中的 SQL Server 节点下，将出现一个名为“本地”的新节点。 这是本地数据库实例。  
  
2.  展开“本地”和“数据库”节点。 请注意具有与 TradeDev 项目同名的数据库的外观。 展开此数据库下的节点。 “数据工具操作”窗口可在“本地”节点数据库上显示正在进行的扩展/导入操作的状态。 请注意，这些节点不包含我们在以前的过程中创建的任何表和实体。  
  
3.  按 F5 调试 TradeDev 数据库项目。  
  
    默认情况下，SSDT 将使用本地数据库服务器实例来调试数据库项目。 在这种情况下，SSDT 将首先尝试生成该项目，如果没有错误，该项目（及其实体）将部署到本地数据库中。 如果您在以后调试相同的项目，SSDT 将检测到您在上次调试会话后进行的任何更改，并且仅将这些更改部署到本地数据库。  
  
4.  再次展开“本地”数据库服务器中 TradeDev 下的节点。 这一次，注意到表、视图和函数已部署到该本地数据库服务器。  
  
5.  右键单击 TradeDev 节点并选择“新建查询”。  
  
6.  在脚本窗格中，粘贴以下代码并单击“执行查询”按钮以便运行该查询。  
  
    ```  
    select * from dbo.GetProductsBySupplier(1)  
    ```  
  
7.  “消息”窗格将显示“(0 行受影响)”并且“结果”窗格将不返回任何行。 这是因为我们对本地数据库进行查询，而非对包含实际数据的连接的数据库进行查询。  
  
    可以通过右键单击这个本地 TradeDev 数据库下方的 Products 表，然后选择“查看数据”，对此进行确认。 请注意，该表是空的。  
  
### <a name="to-replicate-real-data-to-the-local-database"></a>将实际数据复制到本地数据库  
  
1.  在“SQL Server 对象资源管理器”中，展开连接的 SQL Server 实例并找到 TradeDev 数据库。  
  
    右键单击 Suppliers 表并选择“查看数据”。  
  
2.  单击数据编辑器顶部的“脚本”按钮（从右边数第二个按钮）。 从该脚本中复制 `INSERT` 语句。  
  
3.  展开“本地”服务器实例并右键单击 TradeDev 节点，然后选择“新建查询”。  
  
4.  将 `INSERT` 语句粘贴到此查询窗口中并执行查询。  
  
5.  重复上述步骤，将数据从连接的 TradeDev 数据库的 Products 和 Fruits 表复制到本地 TradeDev 数据库。  
  
6.  右键单击该“本地”服务器实例，然后选择“刷新”。 使用“查看数据”对这些表进行检查，以便确认已填充该本地数据库。  
  
7.  右键单击本地服务器实例的 TradeDev 节点，然后选择“新建查询”。  
  
8.  在脚本窗格中，粘贴以下代码并单击“执行查询”按钮以便运行该查询。  
  
    ```  
    select * from dbo.GetProductsBySupplier(1)  
    ```  
  
9. 在 Transact\-SQL 编辑器窗格下的“结果”窗格中，将看到返回 `Products` 表的 Apples 和 Potato Chips 行。  
  
