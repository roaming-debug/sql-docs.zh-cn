---
title: 删除对象和解析依赖关系
description: 了解如何重命名或删除数据库对象。 查看哪些依赖对象 SSDT 自动更新，以及需要更新哪些依赖关系。
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
f1_keywords:
- Microsoft.VisualStudio.Data.Tools.Project.HelpKeywords.SqlProjectDropDatabaseConfirmationDialog
- sql.data.tools.dropdatabaseconfirmation.dialog
- sql.data.tools.dropmultipledatabasesconfirmation.dialog
ms.assetid: fb31c2b1-ca4f-4e11-a0b6-5c26430f1c8c
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 72e9c09651d473c25b2df84e0b6ccecee0964959
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018227"
---
# <a name="how-to-delete-objects-and-resolve-dependencies"></a>如何：删除对象和解析依赖关系

在 SQL Server 对象资源管理器中重命名或删除某一对象时，SQL Server Data Tools 将自动检测其所有依赖对象，并且将准备 ALTER 脚本以便根据需要重命名或删除这些依赖对象。  
  
> [!WARNING]  
> 下面的过程使用了前面[连接的数据库开发](../ssdt/connected-database-development.md)一节介绍的过程中创建的实体。  
  
### <a name="to-delete-a-database"></a>删除数据库  
  
1.  右键单击“SQL Server 对象资源管理器”中的某个数据库，然后选择“删除”。  
  
2.  在“删除数据库”对话框中接受所有默认设置，然后单击“确定”。  
  
### <a name="to-rename-a-table"></a>重命名表  
  
1.  请确保 `Customer` 表未在表设计器或 Transact\-SQL 编辑器中打开。  
  
2.  展开 SQL Server 对象资源管理器中的“表”节点。 右键单击 Customer 表，然后选择“重命名”。  
  
3.  将表名称更改为 Customers，然后按 ENTER 键。  
  
4.  请注意，该工具将立即代表你调用“数据库更新”操作。 SSDT 仍将代表您调用 sp_rename 存储过程以便重命名该表。 如果存在诸如外键约束之类的任何依赖对象，则这些对象也将更新。  
  
    > [!WARNING]  
    > SSDT 不自动更新基于脚本的依赖项，例如对来自视图的表或存储过程的引用。 重命名后，可以使用“错误列表”窗格找到其他所有依赖项，并手动修复它们。  
  
5.  在执行了上一过程中的步骤后应用更改，即[如何：使用 Power Buffer 更新连接的数据库](../ssdt/how-to-update-a-connected-database-with-power-buffer.md)。  
  
6.  再次右键单击“SQL Server 对象资源管理器”中的“Customers”表，然后选择“查看数据”。 请注意，在重命名操作后表数据将保持不变。  
  
7.  右键单击 Products 表，然后选择“查看代码”。 请注意，外键引用已自动更新为 `REFERENCES [dbo].[Customers] ([Id])` 以便反映此重命名操作。  
  
### <a name="to-delete-a-table"></a>删除表  
  
1.  右键单击“SQL Server 对象资源管理器”中的“Customers”表，然后选择“删除”。  
  
2.  在“预览数据库更新”对话框的“用户操作”下，请注意 SSDT 已标识了所有依赖对象，在此情况下，依赖对象是将删除的外键引用。  
  
3.  单击“更新数据库”。  
  
4.  右键单击“SQL Server 对象资源管理器”中的“Products”表，然后选择“查看代码”。 请注意，对 `Customers` 表的外键引用已消失。  
  
    > [!WARNING]  
    > 如果在删除操作发生时已在表设计器或 Transact\-SQL 编辑器中打开了 Products 表，则该表将不会自动刷新以显示外键引用的删除。 此外，与无法解析的引用有关的错误可能会出现在“错误列表”中。 若要解决此问题，请关闭表设计器或 Transact\-SQL 编辑器，然后重新打开 Products 表。  
  
