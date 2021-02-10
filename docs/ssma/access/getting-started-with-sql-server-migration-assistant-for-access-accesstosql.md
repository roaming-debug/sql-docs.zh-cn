---
title: 开始使用 Access SQL Server 迁移助手 |Microsoft Docs
description: 开始使用 SSMA 将 Access 数据库对象转换为 SQL Server 或 Azure SQL 数据库对象、上传生成的对象和迁移数据。
ms.prod: sql
ms.custom: ''
ms.date: 08/15/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- error list pane
- getting started
- menus
- metadata explorers
- output pane
- toolbars
- user interface
- user interface overview
ms.assetid: 462a731f-08f1-44e1-9eeb-4deac6d2f6c5
author: nahk-ivanov
ms.author: alexiva
manager: alexiva
ms.openlocfilehash: e05bc860ec1cc9f3703a1d1c17441c4e1ad3de15
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100044597"
---
# <a name="getting-started-with-sql-server-migration-assistant-for-access-accesstosql"></a>SQL Server 迁移助手访问 (AccessToSQL) 入门
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 迁移助手 (SSMA) 的访问权限允许您快速将 Access 数据库对象转换为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 AZURE Sql 数据库对象，将生成的对象上传到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 Azure sql 数据库，以及将数据从 Access [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 azure sql 数据库迁移到 azure sql database。 如有必要，还可以将 Access 表链接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 AZURE Sql 数据库表，以便可以继续使用现有的 access 前端应用程序 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 Azure sql 数据库。  
  
本主题介绍了安装过程，并帮助你熟悉 SSMA 用户界面。  
  
## <a name="installing-ssma"></a>安装 SSMA  
若要使用 SSMA，首先必须在能够访问要迁移的数据库的计算机和 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 AZURE SQL 数据库的目标实例上安装 SSMA 客户端程序。 有关安装说明，请参阅 [安装 SQL Server 迁移助手以便访问 &#40;AccessToSQL&#41;](../../ssma/access/installing-sql-server-migration-assistant-for-access-accesstosql.md)。  
  
若要启动 SSMA，请单击 " **开始**"，指向 " **所有程序**"，指向 " **SQL Server 迁移助手进行访问**"，然后选择 " **SQL Server 迁移助手进行访问**"。  
  
## <a name="using-ssma"></a>使用 SSMA  
安装 SSMA 后，在使用该工具将 Access 数据库迁移到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 AZURE SQL 数据库之前，有助于熟悉 SSMA 用户界面。 下图显示了 SSMA 用户界面，包括元数据资源管理器、元数据、工具栏、输出窗格和错误列表窗格：  
  
![用于访问图形用户界面的 SSMA](../../ssma/access/media/ssmaforaccessgui.gif "用于访问图形用户界面的 SSMA")  
  
若要开始迁移，请创建新项目，然后将 Access 数据库添加到 Access 元数据资源管理器。 然后，你可以在 Access 元数据资源管理器中右键单击对象，以执行如下任务：
- 将 Access 数据库对象的清单导出到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 AZURE SQL 数据库。
- 创建评估转换为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 AZURE SQL 数据库的报表。
- 将访问架构转换为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 AZURE SQL 数据库架构。

你还可以使用工具栏和菜单执行这些任务。  
  
还必须连接到的实例 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 成功连接后， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 元数据资源管理器中将显示数据库的层次结构。 将访问架构转换为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 架构后，可以在元数据资源管理器中选择这些已转换的架构 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，然后将架构加载到中 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
如果已从 "新项目的迁移到" 对话框中选择了 "Azure SQL 数据库"，则必须连接到 Azure SQL 数据库。 成功连接后，azure sql 数据库数据库的层次结构将出现在 Azure SQL 数据库元数据资源管理器中。 将访问架构转换为 Azure SQL 数据库架构后，可以在 Azure SQL 数据库元数据资源管理器中选择这些已转换的架构，然后将架构加载到中 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
将转换后的架构加载到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 AZURE Sql 数据库后，可以返回到访问元数据资源管理器，并将数据从 access 数据库迁移到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 Azure sql 数据库数据库。 如有必要，还可以将 Access 表链接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 AZURE SQL 数据库表。  
  
有关这些任务以及如何执行这些任务的详细信息，请参阅以下主题：  
  
-   [准备要迁移的 Access 数据库](preparing-access-databases-for-migration-accesstosql.md)  
  
-   [将 Access 数据库迁移到 SQL Server](migrating-access-databases-to-sql-server-azure-sql-db-accesstosql.md)  
  
-   [将访问应用程序链接到 SQL Server](linking-access-applications-to-sql-server-azure-sql-db-accesstosql.md)  
  
以下部分介绍 SSMA 用户界面的功能。  
  
### <a name="metadata-explorers"></a>元数据资源管理器  
SSMA 包含两个元数据资源管理器，可用于在 Access 和 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 AZURE SQL 数据库数据库上浏览和执行操作。  
  
#### <a name="access-metadata-explorer"></a>Access 元数据资源管理器  
访问元数据资源管理器显示有关已添加到项目的 Access 数据库的信息。 添加 Access 数据库时，SSMA 将检索有关该数据库的元数据，这是 Access 元数据资源管理器中可用的元数据。  
  
您可以使用 Access 元数据资源管理器执行以下任务：  
  
-   浏览每个 Access 数据库中的表。  
  
-   选择要转换的对象，并将对象转换为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 语法。 有关详细信息，请参阅 [转换 Access 数据库对象](converting-access-database-objects-accesstosql.md)。  
  
-   选择用于数据迁移的对象，并将这些对象中的数据迁移到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 有关详细信息，请参阅将 [访问数据迁移到 SQL Server](migrating-access-data-into-sql-server-azure-sql-db-accesstosql.md)。  
  
-   链接和取消链接访问与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 表。  
  
#### <a name="sql-server-or-azure-sql-database-metadata-explorer"></a>SQL Server 或 Azure SQL 数据库元数据资源管理器  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 Azure SQL 数据库元数据资源管理器显示有关实例 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 AZURE Sql 数据库的信息。 当你连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 AZURE SQL 数据库的实例时，SSMA 会检索有关该实例的元数据，并将其存储在项目文件中。  
  
你可以使用 SQL Server 或 Azure SQL 数据库的元数据资源管理器选择转换后的 Access 数据库对象，并将这些对象)  (同步到实例 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 AZURE SQL 数据库。  
  
有关详细信息，请参阅将 [转换的数据库对象加载到 SQL Server](loading-converted-database-objects-into-sql-server-accesstosql.md)。  
  
### <a name="metadata"></a>元数据  
每个元数据资源管理器右侧的选项卡都是描述所选对象的选项卡。 例如，如果在 "访问元数据资源管理器" 中选择一个表，将显示以下四个选项卡： **表**、 **类型映射**、 **属性** 和 **数据**。 如果在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 元数据资源管理器中选择表，将显示以下三个选项卡： **表**、 **SQL** 和 **数据**。  
  
大多数元数据设置为只读。 但是，您可以更改以下元数据：  
  
-   在 Access 元数据资源管理器中，您可以更改类型映射。 在创建报表或转换架构之前，请务必进行这些更改。  
  
-   在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] "元数据资源管理器" 中，您可以在 "**表**" 选项卡上更改表和索引属性。将架构加载到之前进行这些更改。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 有关详细信息，请参阅 [转换 Access 数据库对象](converting-access-database-objects-accesstosql.md)。  
  
### <a name="toolbars"></a>工具栏  
SSMA 有两个工具栏：项目工具栏和迁移工具栏。  
  
#### <a name="the-project-toolbar"></a>项目工具栏  
项目工具栏包含用于处理项目、添加 Access 数据库文件以及连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 AZURE SQL 数据库的按钮。 这些按钮类似于 " **文件** " 菜单上的命令。  
  
#### <a name="the-migration-toolbar"></a>迁移工具栏  
迁移工具栏包含以下命令：  
  
|按钮|函数|  
|----------|------------|  
|**转换、加载和迁移**|转换 Access 数据库，将转换后的对象加载到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 AZURE Sql 数据库中，并将数据迁移到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 Azure sql 数据库，所有操作均分为一步。|  
|**创建报表**|将所选访问架构转换为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 AZURE SQL 数据库语法，然后创建显示转换成功的报表。<br /><br />仅当在 "访问元数据资源管理器" 中选择了对象时，此命令才可用。|  
|**转换架构**|将所选访问架构转换为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 AZURE SQL 数据库架构。<br /><br />仅当在 "访问元数据资源管理器" 中选择了对象时，此命令才可用。|  
|**迁移数据**|将数据从 Access 数据库迁移到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 AZURE SQL 数据库。 在运行此命令之前，必须将访问架构转换为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 AZURE Sql 数据库架构，然后将对象加载到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 Azure sql 数据库。<br /><br />仅当在 "访问元数据资源管理器" 中选择了对象时，此命令才可用。|  
|**Stop**|停止当前进程，如将对象转换为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 AZURE SQL Database 语法。|  
  
### <a name="menus"></a>菜单  
SSMA 包含以下菜单：  
  
|菜单|说明|  
|--------|---------------|  
|**File**|包含用于迁移向导、使用项目、添加和删除 Access 数据库文件以及连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 AZURE SQL 数据库的命令。|  
|**编辑**|包含用于查找和处理详细信息页中的文本（例如 [!INCLUDE[tsql](../../includes/tsql-md.md)] 从 SQL 详细信息窗格复制）的命令。 若要打开 " **管理书签** " 对话框，请在 "编辑" 菜单上单击 "管理书签"。 在对话框中，您将看到现有书签的列表。 您可以使用对话框右侧的按钮来管理书签。|  
|**查看**|包含 **同步元数据** 资源管理器命令。 这会同步 Access 元数据资源管理器和 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 AZURE SQL 数据库元数据资源管理器之间的对象。 还包含用于显示和隐藏 " **输出** " 和 " **错误列表** " 窗格以及用于管理布局的选项 **布局** 的命令。|  
|**工具**|包含用于创建报表、导出数据、迁移对象和数据、链接表，并提供对 "全局" 和 "项目设置" 对话框的访问的命令。|  
|**帮助**|提供对 SSMA 帮助和的 " **关于** " 对话框的访问。|  
  
### <a name="output-pane-and-error-list-pane"></a>输出窗格和错误列表窗格  
" **视图** " 菜单提供用于切换 "输出" 窗格和 "错误列表" 窗格的可见性的命令：  
  
-   在对象转换、对象同步和数据迁移期间，"输出" 窗格将显示来自 SSMA 的状态消息。  
  
-   "错误列表" 窗格在列表中显示可以排序的错误、警告和信息性消息。  
  
## <a name="see-also"></a>另请参阅  
[将 Access 数据库迁移到 SQL Server](migrating-access-databases-to-sql-server-azure-sql-db-accesstosql.md)  
  
