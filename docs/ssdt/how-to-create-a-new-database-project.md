---
title: 创建新的数据库项目
description: 了解如何创建新的数据库项目。 查找如何将架构从现有数据库导入新项目。
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
f1_keywords:
- sql.data.tools.dbprojectwizard.importschema
- sql.data.tools.SqlProjectImportDatabaseDialog.dialog
- sql.data.tools.importscriptwizard.welcome
- sql.data.tools.importscriptwizard.summary
- sql.data.tools.SqlProjectImportDatabaseSummaryDialog.dialog
- sql.data.tools.importscriptwizard.fileselection
ms.assetid: 0b7883fa-b6e1-4ccf-b1d8-f522fd03a59d
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 6039138eda38f67aae2a1f4770ebc93380087607
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018367"
---
# <a name="how-to-create-a-new-database-project"></a>如何：创建新的数据库项目

您可以创建一个新的数据库项目，并从现有数据库、.sql 脚本文件或数据层应用程序 (.dacpac) 中导入数据库架构。 然后可以调用可用于所连接的数据库开发的相同可视化设计器工具（Transact\-SQL 编辑器、表设计器），对脱机数据库项目进行更改，然后将更改发布回生产数据库。 这些更改也可以另存为脚本以便在以后发布。 使用“项目属性”窗格，可以将目标平台更改为不同的 SQL Server 版本（包括 SQL Azure）。  
  
下面的两个过程通过创建一个新的数据库项目并从现有数据库导入架构，大体上实现同样的目标。 每个数据库对象都将表示为“解决方案资源管理器”中的 SQL 脚本文件 (.sql)。 有关从快照导入数据库架构的详细信息，请参阅[如何：创建项目的快照](../ssdt/how-to-create-a-snapshot-of-a-project.md)。  
  
> [!WARNING]  
> 下面的过程利用在[连接的数据库开发](../ssdt/connected-database-development.md)一节的前面的过程中创建的实体。  
  
### <a name="to-create-a-new-database-project-off-a-connected-database"></a>脱离连接的数据库创建一个新数据库项目  
  
1.  右键单击“SQL Server 对象资源管理器”中的“TradeDev”节点，然后选择“创建新项目”。  
  
2.  在“导入数据库”对话框中，请注意，“源数据库连接”设置已由你在“SQL Server 对象资源管理器”中所选的数据库预定义。 在“目标项目”设置中，将项目名称更改为“TradeDev”。  
  
3.  在“导入设置”部分，请注意用于导入特定对象和设置的选项，以及每个架构和/或对象类型的创建文件夹。 对于你的所有数据库对象的组织层次结构，接受所有默认设置，然后单击“启动”。  
  
4.  “导入数据库”对话框显示进度栏，并显示正在导入对象 SSDT 的列表。 导入操作完成后，单击“完成”以退出最后一个屏幕。  
  
5.  检查“解决方案资源管理器”中的层次结构。 展开“dbo”文件夹，你将看到单独的“函数”、“表”和“视图”文件夹。 请注意，表和函数将按其架构文件夹分组。  
  
6.  双击“表”下的“Products.sql”。 将打开“表设计器”，显示列网格中的表的可视化解释，以及脚本窗格中的表的脚本定义。 这与我们在[连接的数据库开发](../ssdt/connected-database-development.md)部分中看到的内容完全相同。  
  
7.  为“CustomerId”列取消选中“允许 Null 值”框。 按 Ctrl+S 保存该文件。  
  
8.  在“解决方案资源管理器”中右键单击“TradeDev”项目，然后选择“生成”以便生成数据库项目。  
  
    生成操作的结果可在输出窗口中看到。  
  
### <a name="to-create-a-new-project-and-import-existing-database-schema"></a>创建新项目并导入现有数据库架构  
  
1.  依次单击“文件”、“新建”和“项目”。 在“新建项目”对话框的左窗格中选择“SQL Server”。 请注意，只有一种类型的数据库项目：“SQL Server 数据库项目”。 与以前的 Visual Studio 版本不同，没有特定于平台的项目。 在创建了项目后，你将能够在“项目设置”对话框中设置你的目标平台。 此类任务将在主题[如何：更改目标平台并发布数据库项目](../ssdt/how-to-change-target-platform-and-publish-a-database-project.md)中予以介绍。  
  
2.  将项目的名称更改为“TradeDev”，然后单击“确定”以便创建新项目。  
  
3.  在“解决方案资源管理器”中右键单击新创建的“TradeDev”项目，选择“导入”，然后选择“数据库”。  
  
    “导入数据库”对话框随即打开。 在“源数据库连接”部分中，单击“选择数据库”，然后选择“TradeDev”。 如果“TradeDev”在下拉列表中不存在，请使用“新建连接”按钮以编辑连接属性。  
  
4.  在“导入设置”部分，请注意用于导入特定对象和设置的选项，以及每个架构和/或对象类型的创建文件夹。 对于你的所有数据库对象的组织层次结构，接受所有默认设置，然后单击“启动”。  
  
5.  “导入数据库”对话框显示进度栏，并显示正在导入对象 SSDT 的列表。 导入操作完成后，单击“完成”以退出最后一个屏幕。  
  
6.  检查“解决方案资源管理器”中的层次结构。 展开“dbo”文件夹，你将看到单独的“函数”、“表”和“视图”文件夹。 请注意，表和函数将按其架构文件夹分组。  
  
7.  双击“表”下的“Products.sql”。 将打开“表设计器”，显示列网格中的表的可视化解释，以及脚本窗格中的表的脚本定义。 这与我们在[连接的数据库开发](../ssdt/connected-database-development.md)部分中看到的内容完全相同。  
  
8.  为“CustomerId”列取消选中“允许 Null 值”框。 按 Ctrl+S 保存该文件。  
  
9. 在“解决方案资源管理器”中右键单击“TradeDev”项目，然后选择“生成”以便生成数据库项目。  
  
## <a name="see-also"></a>另请参阅  
[如何：更改目标平台并发布数据库项目](../ssdt/how-to-change-target-platform-and-publish-a-database-project.md)  
  
