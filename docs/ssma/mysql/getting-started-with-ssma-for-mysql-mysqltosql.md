---
title: 与 SSMA for MySQL (MySQLToSQL) 入门 |Microsoft Docs
description: 了解 MySQL 安装过程的 SQL Server 迁移助手 (SSMA) ，并熟悉 SSMA 用户界面。
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Getting started, MySQL metadata explorer
- Getting started, SQL Server or SQL Azure metadata explorer
- Getting started,Installing and licensing
ms.assetid: 8ebfa061-be6f-4a07-923f-8dc832a82f70
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 9847b7a709fe64f3fcb7fca92ce1f8a4003843e5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100070252"
---
# <a name="getting-started-with-ssma-for-mysql-mysqltosql"></a>SSMA for MySQL 入门 (MySQLToSQL)
利用 MySQL SQL Server 迁移助手 (SSMA) ，可以快速将 MySQL 数据库架构转换为 SQL Server 或 Azure SQL 数据库架构，将生成的架构上传到 SQL Server 或 Azure SQL 数据库，以及将数据从 MySQL 迁移到 SQL Server 或 Azure SQL 数据库。  
  
本主题介绍了安装过程，并帮助你熟悉 SSMA 用户界面。  
  
## <a name="installing-ssma"></a>安装 SSMA  
若要使用 SSMA，首先必须在可以访问源 MySQL 数据库和 SQL Server 或 Azure SQL 数据库的目标实例的计算机上安装 SSMA 客户端程序。 然后，在运行 SSMA 客户端程序的计算机上 (MySQL ODBC 5.1 驱动程序安装 MySQL 提供程序 (受信任) # A3。 有关安装说明，请参阅 [安装 SSMA For MySQL &#40;MySqlToSql&#41;](../../ssma/mysql/installing-ssma-for-mysql-mysqltosql.md)  
  
若要启动 SSMA，请单击 " **开始**"，指向 " **所有程序**"，指向 " **mysql SQL Server 迁移助手**"，然后单击 " **SQL Server 迁移助手**"。  
  
## <a name="ssma-for-mysql-user-interface"></a>用于 MySQL 用户界面的 SSMA  
安装并许可 SSMA 后，可以使用 SSMA 将 MySQL 数据库迁移到 SQL Server 或 Azure SQL 数据库。 在开始之前，它有助于熟悉 SSMA 用户界面。 下图显示了 SSMA 的用户界面，包括元数据资源管理器、元数据、工具栏、输出窗格和 "错误列表" 窗格：  
  
![用于 MySql 图形用户界面的 SSMA](../../ssma/mysql/media/ssmaformysqlgui.gif "用于 MySql 图形用户界面的 SSMA")  
  
若要开始迁移，必须执行以下操作：  
  
1.  创建新项目。  
  
2.  连接到 MySQL 数据库。  
  
3.  成功连接后，mysql 架构将出现在 MySQL 元数据资源管理器中。 右键单击 "MySQL 元数据资源管理器" 中的 "对象"，可执行创建报表以评估到 SQL Server/Azure SQL Database 的转换等任务。  
  
你还可以使用工具栏和菜单执行这些任务。  
  
还必须连接到 SQL Server 的实例。 成功连接后，将在 SQL Server 元数据资源管理器中显示 SQL Server 数据库的层次结构。 将 MySQL 架构转换为 SQL Server 架构后，请在 SQL Server 元数据资源管理器中选择这些已转换的架构，然后将架构与 SQL Server 同步。  
  
如果在 "新建项目" 对话框中选择了 "从迁移到下拉列表中的 Azure SQL 数据库"，则必须连接到 Azure SQL 数据库。 成功连接后，azure sql 数据库数据库的层次结构将出现在 Azure SQL 数据库元数据资源管理器中。 将 MySQL 架构转换为 Azure SQL 数据库架构后，请在 Azure SQL 数据库元数据资源管理器中选择这些已转换的架构，然后将架构与 Azure SQL 数据库同步。  
  
将转换后的架构与 SQL Server 或 Azure SQL 数据库同步后，可以返回到 MySQL 元数据资源管理器，并将数据从 MySQL 架构迁移到 SQL Server 或 Azure SQL 数据库数据库。  
  
有关这些任务以及如何执行这些任务的详细信息，请参阅 [将 MySQL 数据库迁移到 SQL Server-AZURE SQL 数据库 &#40;MySQLToSql&#41;](../../ssma/mysql/migrating-mysql-databases-to-sql-server-azure-sql-db-mysqltosql.md)。  
  
以下部分介绍 SSMA 用户界面的功能。  
  
### <a name="metadata-explorers"></a>元数据资源管理器  
SSMA 包含两个元数据资源管理器，用于浏览 MySQL 和 SQL Server 数据库上的操作。  
  
### <a name="mysql-metadata-explorer"></a>MySQL 元数据资源管理器  
MySQL 元数据资源管理器显示有关 MySQL 架构的信息。 通过使用 MySQL 元数据资源管理器，你可以执行以下任务：  
  
-   浏览每个架构中的对象。  
  
-   选择要转换的对象，然后将对象转换为 SQL Server 语法。 有关详细信息，请参阅将 [MySQL 数据库转换 &#40;MySQLToSQL&#41;](../../ssma/mysql/converting-mysql-databases-mysqltosql.md)  
  
-   选择用于数据迁移的表，然后将这些表中的数据迁移到 SQL Server。 有关详细信息，请参阅将 [MySQL 数据迁移到 SQL Server-AZURE SQL 数据库 &#40;MySQLToSQL&#41;](../../ssma/mysql/migrating-mysql-data-into-sql-server-azure-sql-db-mysqltosql.md)  
  
### <a name="sql-server-or-azure-sql-database-metadata-explorer"></a>SQL Server 或 Azure SQL 数据库元数据资源管理器  
SQL Server 或 Azure SQL 数据库元数据资源管理器显示有关 SQL Server 或 Azure SQL 数据库的实例的信息。 当你连接到 SQL Server 或 Azure SQL 数据库的实例时，SSMA 会检索有关该实例的元数据，并将其存储在项目文件中。  
  
您可以使用此元数据资源管理器选择转换后的 MySQL 数据库对象，然后将这些对象与 SQL Server 或 Azure SQL 数据库的实例同步。  
  
有关详细信息，请参阅 [同步 (MySQL 到 SQL Server/AZURE SQL 数据库) ](./loading-converted-database-objects-into-sql-server-mysqltosql.md)  
  
### <a name="metadata"></a>元数据  
每个元数据资源管理器右侧的选项卡都是描述所选对象的选项卡。 例如，如果在 MySQL 元数据资源管理器中选择一个表，将显示九个选项卡： **表**、 **SQL**、 **类型映射**、 **数据**、 **设置**、 **字符集映射**、 **SQL 模式**、 **属性** 和 **报表**。 仅在创建包含所选对象的报表后，" **报表** " 选项卡才包含信息。 如果在 SQL Server 元数据资源管理器中选择一个表，将显示以下三个选项卡： **表**、 **SQL** 和 **数据**。  
  
大多数元数据设置为只读。 但是，您可以更改以下元数据：  
  
-   在 MySQL 元数据资源管理器中，你可以更改类型映射、字符集映射和 SQL 模式。 若要转换更改的类型映射或字符集映射或 SQL 模式，请在转换架构之前进行更改。  
  
-   在 SQL Server 元数据资源管理器中，您可以在 "表" 选项卡上更改表和索引属性。若要查看 SQL Server 中的这些更改，请在将架构加载到 SQL Server 之前进行这些更改。  
  
在元数据资源管理器中所做的更改将反映在项目元数据中，而不是源或目标数据库中。  
  
### <a name="toolbars"></a>工具栏  
SSMA 有两个工具栏：项目工具栏和迁移工具栏。  
  
### <a name="the-project-toolbar"></a>项目工具栏  
项目工具栏包含用于处理项目、连接到 MySQL 以及连接到 SQL Server 或 Azure SQL 数据库的按钮。 这些按钮类似于 " **文件** " 菜单上的命令。  
  
### <a name="migration-toolbar"></a>迁移工具栏  
下表显示了迁移工具栏命令：  
  
|||  
|-|-|  
|**Button**|**Function**|  
|**创建报表**|将所选 MySQL 对象转换为 SQL Server 或 Azure SQL 数据库对象，然后创建一个报告，该报告显示转换成功的方式。<br /><br />除非在 MySQL 元数据资源管理器中选择了对象，否则将禁用此命令。|  
|**转换架构**|将所选 MySQL 对象转换为 SQL Server 或 Azure SQL 数据库对象。<br /><br />除非在 MySQL 元数据资源管理器中选择了对象，否则将禁用此命令。|  
|**迁移数据**|将数据从 MySQL 数据库迁移到 SQL Server 或 Azure SQL 数据库。 在运行此命令之前，必须将 MySQL 架构转换为 SQL Server 或 Azure SQL 数据库架构，然后将对象加载到 SQL Server 或 Azure SQL 数据库。<br /><br />除非在 MySQL 元数据资源管理器中选择了对象，否则将禁用此命令。|  
|**Stop**|停止当前进程。|  
  
### <a name="menus"></a>菜单  
下表显示了 SSMA 菜单。  
  
|||  
|-|-|  
|**菜单**|**说明**|  
|**File**|包含用于处理项目、连接到 MySQL 以及连接到 SQL Server 或 Azure SQL 数据库的命令。|  
|**编辑**|包含用于查找和处理详细信息页中的文本的命令。 若要打开 " **管理书签** " 对话框，请在 "编辑" 菜单上单击 "管理书签"。 在对话框中，您将看到现有书签的列表。 您可以使用对话框右侧的按钮来管理书签。|  
|**查看**|包含 **同步元数据** 资源管理器命令。 这会同步 MySQL 元数据资源管理器与 SQL Server 或 Azure SQL 数据库元数据资源管理器之间的对象。 还包含用于显示和隐藏 " **输出** " 和 " **错误列表** " 窗格以及用于管理布局的选项 **布局** 的命令。|  
|**工具**|包含用于创建报表、转换架构、从数据库刷新、迁移对象和数据，并另存为脚本的命令。 还提供对 **全局设置、"默认项目设置** " 和 " **项目设置** " 对话框的访问。|  
|**帮助**|提供对 SSMA 帮助和的 " **关于** " 对话框的访问。|  
  
### <a name="output-pane-and-error-list-pane"></a>输出窗格和错误列表窗格  
" **视图** " 菜单提供用于切换 "输出" 窗格和 "错误列表" 窗格的可见性的命令：  
  
-   在对象转换、对象同步和数据迁移期间，"输出" 窗格将显示来自 SSMA 的状态消息。  
  
-   "错误列表" 窗格显示可排序列表中的错误、警告和信息性消息。  
  
## <a name="see-also"></a>另请参阅  
[用户界面参考 &#40;MySQLToSQL&#41;](../../ssma/mysql/user-interface-reference-mysqltosql.md)  
[将 MySQL 数据迁移到 SQL Server-Azure SQL 数据库 &#40;MySQLToSQL&#41;](../../ssma/mysql/migrating-mysql-data-into-sql-server-azure-sql-db-mysqltosql.md)  
