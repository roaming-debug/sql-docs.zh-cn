---
title: 调试存储过程
description: 了解如何使用 Transact-SQL 调试器以交互方式调试存储过程。 了解如何显示 SQL 调用堆栈、局部变量和参数。
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
f1_keywords:
- SQL.DATA.TOOLS.EXECUTESTOREDPROCEDURE.DIALOG
ms.assetid: e3c8707f-0f6b-4265-8a5a-81f079330b52
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 6c34db253855127451f375045e51418850bc824a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018237"
---
# <a name="how-to-debug-stored-procedures"></a>如何：调试存储过程

使用 Transact\-SQL 调试器，你可以通过为 SQL 存储过程显示 SQL 调用堆栈、局部变量和参数，以交互方式调试存储过程。 与其他编程语言中的调试一样，你可以在调试 Transact\-SQL 脚本的同时查看和修改局部变量和参数、查看全局变量以及控制和管理断点。  
  
本示例说明如何通过单步执行创建和调试 Transact\-SQL 存储过程。  
  
> [!WARNING]  
> 以下过程利用在[连接的数据库开发](../ssdt/connected-database-development.md)和[面向项目的脱机数据库开发](../ssdt/project-oriented-offline-database-development.md)部分中的过程中创建的实体。  
  
### <a name="to-debug-stored-procedures"></a>调试存储过程  
  
1.  在“解决方案资源管理器”中，右键单击“TradeDev”项目，选择“添加”，然后选择“存储过程”。 将这个新的存储过程命名为 AddProduct，然后单击“添加”。  
  
2.  将以下代码粘贴到该存储过程中。  
  
    ```  
    CREATE PROCEDURE [dbo].[AddProduct]  
    @id int,  
    @name nvarchar(128)  
    AS  
    INSERT INTO [dbo].[Product] (Id, Name) VALUES (@id, @name)  
    ```  
  
3.  按 F5 生成和部署该项目。  
  
4.  在 SQL Server 对象资源管理器的的“本地”节点下，右键单击“TradeDev”数据库，然后选择“新建查询”。  
  
5.  将下面的代码粘贴到查询窗口中。  
  
    ```  
    EXEC [dbo].[AddProduct] 50, N'Contoso';  
    GO  
    ```  
  
6.  单击左窗口边距以便向 `EXEC` 语句添加断点。  
  
7.  按下 Transact\-SQL 编辑器工具栏中的绿色箭头按钮上的下拉箭头，然后选择“使用调试器执行”，以便使用调试执行查询。  
  
8.  或者，你可以从 SQL Server 对象资源管理器开始调试。 右键单击 AddProduct 存储过程（位于 “本地” -> “TradeDev”数据库 -> “可编程性” -> “存储过程”下）。 选择“调试过程...”。如果对象需要参数，则会出现“调试过程”对话框，显示一个包含各个参数行的表。 表中的每一行都包含参数名称列和参数值列。 输入各个参数的值，再单击“确定”。  
  
9. 请确保“本地”窗口打开。 如果未打开，则单击“调试”菜单，选择“窗口”和“本地”。    
  
10. 按 F11 键逐行执行该查询。 注意，存储过程的参数及其各自的值显示在“局部变量”窗口中。 或者，将鼠标指针悬停在 `INSERT` 子句中的 `@name` 参数上方，你将看到要传递给它的 Contoso 值。  
  
11. 在文本框中单击 Contoso。 键入 Fabrikam，然后按下 ENTER 以便在调试时更改 `name` 变量的值。 还可以在“局部变量”窗口中更改其值。 请注意，该参数的值现在显示为红色，表示它已经更改。  
  
12. 按 F10 键逐行执行其余代码。  
  
13. 在 SQL Server 对象资源管理器中，刷新 TradeDev 数据库节点以查看 Product 表的数据视图中的新内容。  
  
14. 在 SQL Server 对象资源管理器中，在“本地”节点下，找到 TradeDev 数据库的 Product 表。  
  
15. 右键单击 Product 表，然后选择“查看数据”。 请注意，新行已添加到该数据库中。  
  
