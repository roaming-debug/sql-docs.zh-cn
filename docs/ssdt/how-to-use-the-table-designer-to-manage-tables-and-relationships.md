---
title: 使用表设计器管理表和关系
description: 熟悉表设计器。 请参阅如何使用此工具创建和编辑数据库表结构，以及如何用它来查看数据库对象之间的关系。
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
f1_keywords:
- sql.data.tools.design.table.columnspecification.index.dialog
- sql.data.tools.design.table.columnsgrid.view
- sql.data.tools.design.table.scriptpanel
ms.assetid: 322a2903-d7a6-4f52-9048-1bd413b4c799
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: a71009351c187bdda50c00b4f3b00bfee0fd2acf
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100017997"
---
# <a name="how-to-use-the-table-designer-to-manage-tables-and-relationships"></a>如何：使用表设计器管理表和关系

在为 Transact\-SQL 数据库创建和编辑表结构（包括特定于表的编程对象）时，表设计器提供与 SQL Server 编辑器相同的视觉体验。  当为连接的数据库或项目创建新表时，或者当你在 SQL Server 对象资源管理器或解决方案资源管理器中双击以便编辑某一表时，表设计器将启动。  
  
该设计器由列网格、脚本窗格和上下文窗格构成。 列网格将列出表中的所有列。 您可以添加、编辑和删除此网格中的列。  上下文窗格将为你提供表定义（键、索引、约束、触发器等）的逻辑视图，使你可以选择一个对象以便突出显示其与单独列的关系。 你还可以将新对象添加到此窗格中的表，并在属性网格中编辑选定对象的属性。 脚本窗格显示表结构的定义，并且突出显示上下文窗格或列网格中所选对象的脚本。 您可以在视图中与列网格和上下文窗格并排的情况下编辑脚本。 在这三个窗格中任何一个窗格的任何更改都将立即传播到其他两个窗格。  
  
> [!WARNING]  
> 以下过程利用在[连接的数据库开发](../ssdt/connected-database-development.md)和[面向项目的脱机数据库开发](../ssdt/project-oriented-offline-database-development.md)部分中的过程中创建的实体。  
  
### <a name="to-create-a-new-table"></a>创建新表  
  
1.  打开您在以前的过程中已在处理的 TradeDev 项目。  
  
2.  在“解决方案资源管理器”中，展开“dbo”文件夹，右键单击“表”文件夹并选择“添加”，然后选择“表”。   
  
3.  将这个新表命名为“Shipper”，然后单击“添加”。  
  
4.  表设计器随即打开。 在列网格中，向该表添加名为 ShipperName 且数据类型为 int 的一个新列。  
  
5.  请注意，你还可以在“属性”窗口中编辑列的属性。 单击 ShipperName 列，在“属性”窗口中，将该列的“数据类型”更改为 nvarchar，将“长度”更改为 128。 请注意，在您将焦点移出该字段时，设计器的脚本窗格和列网格将自动更新以便反映您的更改。  
  
### <a name="to-create-a-new-foreign-key-constraint"></a>创建新的外键约束  
  
1.  在设计器的上下文窗格中右键单击“外键”节点，然后选择“添加新的外键”。  
  
2.  请注意，节点计数将自动递增 1。 按 ENTER 键接受该约束的默认名称。  
  
3.  使用以下内容替换脚本窗格中该约束的默认定义。  
  
    ```  
    CONSTRAINT [FK_Shipper_Products] FOREIGN KEY ([Id]) REFERENCES [dbo].[Products]([Id])  
    ```  
  
    请注意，为脱机项目创建和编辑数据库实体的体验与对连接的数据库执行这些任务完全相同。  
  
## <a name="see-also"></a>另请参阅  
[如何：使用表设计器创建数据库对象](../ssdt/how-to-create-database-objects-using-table-designer.md)  
  
