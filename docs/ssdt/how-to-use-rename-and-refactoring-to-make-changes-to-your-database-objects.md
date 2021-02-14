---
title: 重命名和重构以更改数据库对象
description: 了解 Transact-SQL 编辑器中的“重命名”和“重构”菜单。 查看如何重命名类型、切换对象架构、展开通配符和完全限定名称。
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
f1_keywords:
- sql.data.tools.dbrefactoring.previewdialog
- sql.data.tools.editor.howto.refactoring
- sql.data.tools.dbrefactoring.renamedialog
- sql.data.tools.dbrefactoring.moveschemadialog
- sql.data.tools.dbrefactoring.renameserverdatabasedialog
ms.assetid: f35520e6-8e6e-47b1-87a3-22c0cf2cabdb
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 8fd846301467dbbcbf08aa6bf4a02e52fb467c4b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018017"
---
# <a name="how-to-use-rename-and-refactoring-to-make-changes-to-your-database-objects"></a>如何：使用重命名和重构更改数据库对象

通过 Transact\-SQL 编辑器中的“重构”上下文菜单，可以重命名对象或将对象移到不同架构，并且在提交更改前预览所有受影响的区域。 也可以使用“重构”菜单完全限定对数据库对象的所有引用，或者扩展你的数据库项目的 `SELECT` 语句中的任何通配符。  
  
> [!NOTE]  
> 以下过程利用在[连接的数据库开发](../ssdt/connected-database-development.md)和[面向项目的脱机数据库开发](../ssdt/project-oriented-offline-database-development.md)部分中的之前过程中创建的实体。  
  
### <a name="to-rename-a-type"></a>重命名类型  
  
1.  在“解决方案资源管理器”中右键单击 Products 表 (Products.sql)，然后选择“查看代码”以便在 Transact\-SQL 编辑器中打开该脚本。  
  
2.  在脚本中右键单击 `[Products]`，然后选择“重构”和“重命名”。  
  
3.  在“新名称”字段中，将其更改为 Product。 保持“预览更改”选项选中，然后单击“确定”。  
  
4.  在下一个屏幕中，您将能够预览将会受此重命名操作影响的脚本的列表。 具体来说，引用 `Products` 的所有位置都将突出显示。 这非常类似于前一过程中的“查找所有引用”任务。 单击顶部窗格中的任意内容，然后在底部窗格的脚本中查看实际更改（以绿色突出显示）。  
  
5.  单击“应用”。  
  
6.  对于已在表设计器或 Transact\-SQL 编辑器中打开的脚本文件，请注意 Transact\-SQL 编辑器已用左侧的绿色条突出显示了发生了更改的位置。  
  
7.  请注意在“解决方案资源管理器”中添加了 TradeDev.refactorlog。 双击将其打开。 它包含此会话中所有更改的 XML 表示形式。  
  
8.  按 F5 以生成该项目并将该项目部署到本地数据库中。  
  
9. 右键单击“SQL Server 对象资源管理器”中“本地”下的“TradeDev”数据库，然后选择“刷新”。  
  
10. 展开“表”，你会发现，Products 表已重命名。  
  
11. 右键单击 Product，然后选择“查看数据”。 请注意，现有的数据保持不变，而与该重命名操作无关。  
  
### <a name="to-expand-wildcards"></a>展开通配符  
  
1.  在“解决方案资源管理器”中展开“函数”节点，然后双击 GetProductsBySupplier.sql。  
  
2.  将光标放置于此行中的星号上，然后右击。 选择“重构”和“展开通配符”。  
  
    ```  
    SELECT * from Product p  
    ```  
  
3.  在“预览更改”对话框中，在顶部窗格上单击 `SELECT * from Product p` 以突出显示它。  
  
4.  在下面的“预览更改”窗格中，请注意在脚本中 `*` 已展开为以下形式。  
  
    ```  
    [Id], [Name], [ShelfLife], [SupplierId], [CustomerId]  
    ```  
  
5.  单击“应用”按钮。  请注意，包含由展开操作产生的更改的行再次用左侧的绿色条突出显示。  
  
### <a name="to-fully-qualify-database-object-names"></a>完全限定数据库对象名称  
  
1.  请确保 GetProductsBySupplier.sql 仍在 Transact\-SQL 编辑器中打开。  
  
2.  将光标放置于此行中的 `Product` 上，然后右键单击。 选择“重构”和“完全限定名称”。  
  
    ```  
    SELECT [Id], [Name], [ShelfLife], [SupplierId], [CustomerId] from Product p  
    ```  
  
3.  在“预览更改”对话框中，单击“应用”按钮。  请注意，所有的对象引用都已更新，以便包括对象架构的名称和父对象的名称（如果该对象具有父对象）。  
  
    ```  
    SELECT [p].[Id], [p].[Name], [p].[ShelfLife], [p].[SupplierId], [p].[CustomerId] from [dbo].[Product] p  
    ```  
  
### <a name="to-move-schema"></a>移动架构  
  
1.  右键单击要移动的对象。 选择“重构”和“移动架构”。  
  
2.  在“新架构”列表中，单击要将对象移入的架构名称。 单击“确定”。  
  
    如果选中“预览更改”复选框，则显示“预览更改”对话框。 否则，将更新对象名称并将对象移到新架构。  
  
