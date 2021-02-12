---
description: SSMA for DB2 (DB2ToSQL) 的入门
title: SSMA for DB2 (DB2ToSQL) 入门Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 48ca32fc-1830-4d1f-add7-480ba5ad02e8
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 1829101a62a3f2602e32b6e4d3535292a90a140d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100069502"
---
# <a name="getting-started-with-ssma-for-db2-db2tosql"></a>SSMA for DB2 (DB2ToSQL) 的入门
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] DB2 的迁移助手 (SSMA) 使你可以快速将 DB2 数据库架构转换为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 架构，将生成的架构上载到中， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 并将数据从 DB2 迁移到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
本主题介绍了安装过程，并帮助你熟悉 SSMA 用户界面。  
  
## <a name="installing-ssma"></a>安装 SSMA  
若要使用 SSMA，首先必须在可以访问源 DB2 数据库和目标实例的计算机上安装 SSMA 客户端程序 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 运行的计算机上的 DB2 OLEDB 提供程序 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 这些组件支持数据迁移和 DB2 系统功能的仿真。 有关安装说明，请参阅 [安装 SSMA FOR DB2 &#40;DB2ToSQL&#41;](../../ssma/db2/installing-ssma-for-db2-db2tosql.md)。  
  
若要启动 SSMA，请单击 "**开始**"，指向 "**所有程序**"，指向 " **[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] db2 迁移助手**"，然后单击 " **[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 迁移助手**"。  
  
## <a name="ssma-for-db2-user-interface"></a>DB2 用户界面的 SSMA  
安装 SSMA 后，可以使用 SSMA 将 DB2 数据库迁移到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 在开始之前，它有助于熟悉 SSMA 用户界面。 下图显示了 SSMA 的用户界面，包括元数据资源管理器、元数据、工具栏、输出窗格和 "错误列表" 窗格：  
  
![SSMA 用户界面](../../ssma/db2/media/ssma_db2_ui.png "SSMA 用户界面")  
  
若要开始迁移，必须首先创建一个新项目。 然后，连接到 DB2 数据库。 成功连接后，db2 架构将显示在 DB2 元数据资源管理器中。 然后，您可以在 DB2 元数据资源管理器中右键单击对象，以执行创建评估转换到的报表等任务 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 你还可以使用工具栏和菜单执行这些任务。  
  
还必须连接到的实例 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 成功连接后， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 元数据资源管理器中将显示数据库的层次结构。 将 DB2 架构转换为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 架构后，在元数据资源管理器中选择这些已转换的架构 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，然后将这些架构与同步 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
将转换后的架构与同步之后 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，可以返回到 Db2 元数据资源管理器，并将数据从 db2 架构迁移到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库。  
  
有关这些任务以及如何执行这些任务的详细信息，请参阅 [将 DB2 数据库迁移到 SQL Server &#40;DB2ToSQL&#41;](../../ssma/db2/migrating-db2-databases-to-sql-server-db2tosql.md)。  
  
以下部分介绍 SSMA 用户界面的功能。  
  
### <a name="metadata-explorers"></a>元数据资源管理器  
SSMA 包含两个元数据资源管理器，用于在 DB2 和数据库上浏览和执行操作 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
#### <a name="db2-metadata-explorer"></a>DB2 元数据资源管理器  
DB2 元数据资源管理器显示有关 DB2 架构的信息。 通过使用 DB2 元数据资源管理器，您可以执行以下任务：  
  
-   浏览每个架构中的对象。  
  
-   选择要转换的对象，然后将对象转换为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 语法。 有关详细信息，请参阅将 [DB2 架构转换 &#40;DB2ToSQL&#41;](../../ssma/db2/converting-db2-schemas-db2tosql.md)。  
  
-   选择用于数据迁移的表，然后将这些表中的数据迁移到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 有关详细信息，请参阅 [将 DB2 数据库迁移到 SQL Server &#40;DB2ToSQL&#41;](../../ssma/db2/migrating-db2-databases-to-sql-server-db2tosql.md)。  
  
#### <a name="sql-server-metadata-explorer"></a>SQL Server 元数据资源管理器  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] "元数据资源管理器" 显示有关实例的信息 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 当连接到实例时 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，SSMA 将检索有关该实例的元数据，并将其存储在项目文件中。  
  
您可以使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] "元数据资源管理器" 选择转换后的 DB2 数据库对象，然后将这些对象与的实例同步 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
### <a name="metadata"></a>元数据  
每个元数据资源管理器右侧的选项卡都是描述所选对象的选项卡。 例如，如果您在 DB2 元数据资源管理器中选择一个表，将出现六个选项卡： **表**、 **SQL**、 **类型映射、报表**、 **属性** 和 **数据**。 仅在创建包含所选对象的报表后，" **报表** " 选项卡才包含信息。 如果在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 元数据资源管理器中选择表，将显示三个选项卡： **表**、 **SQL** 和 **数据**。  
  
大多数元数据设置为只读。 但是，您可以更改以下元数据：  
  
-   在 DB2 元数据资源管理器中，可以更改过程和类型映射。 若要转换更改的过程和类型映射，请在转换架构之前进行更改。  
  
-   在 " [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 元数据资源管理器" 中，可以更改 [!INCLUDE[tsql](../../includes/tsql-md.md)] 存储过程的。 若要在中查看这些更改，请在将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 架构加载到之前进行这些更改 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
在元数据资源管理器中所做的更改将反映在项目元数据中，而不是源或目标数据库中。  
  
### <a name="toolbars"></a>工具栏  
SSMA 有两个工具栏：项目工具栏和迁移工具栏。  
  
#### <a name="the-project-toolbar"></a>项目工具栏  
项目工具栏包含用于处理项目、连接到 DB2 以及连接到的按钮 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 这些按钮类似于 " **文件** " 菜单上的命令。  
  
#### <a name="migration-toolbar"></a>迁移工具栏  
下表显示了迁移工具栏命令：  
  
|按钮|函数|  
|------|--------|  
|**创建报表**|将所选 DB2 对象转换为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 语法，然后创建显示转换成功的报表。<br /><br />除非在 DB2 元数据资源管理器中选择了对象，否则将禁用此命令。|  
|**转换架构**|将所选 DB2 对象转换为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 对象。<br /><br />除非在 DB2 元数据资源管理器中选择了对象，否则将禁用此命令。|  
|**迁移数据**|将数据从 DB2 数据库迁移到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 在运行此命令之前，必须将 DB2 架构转换为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 架构，然后将对象加载到中 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。<br /><br />除非在 DB2 元数据资源管理器中选择了对象，否则将禁用此命令。|  
|**Stop**|停止当前进程。|  
  
### <a name="menus"></a>菜单  
下表显示了 SSMA 菜单。  
  
|菜单|说明|  
|----|-----------|  
|**File**|包含用于处理项目、连接到 DB2 以及连接到的命令 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。|  
|**编辑**|包含用于查找和处理详细信息页中的文本（例如 [!INCLUDE[tsql](../../includes/tsql-md.md)] 从 SQL 详细信息窗格复制）的命令。 还包含 " **管理书签** " 选项，您可以在其中查看现有书签的列表。 您可以使用对话框右侧的按钮来管理书签。|  
|**查看**|包含 **同步元数据** 资源管理器命令。 同步 DB2 元数据资源管理器和 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 元数据资源管理器之间的对象。 还包含用于显示和隐藏 " **输出** " 和 " **错误列表** " 窗格以及用于管理布局的选项 **布局** 的命令。|  
|**工具**|包含用于创建报表以及迁移对象和数据的命令。 还提供对 " **全局设置** " 和 " **项目设置** " 对话框的访问。|  
|**帮助**|提供对 SSMA 帮助和的 " **关于** " 对话框的访问。|  
  
### <a name="output-pane-and-error-list-pane"></a>输出窗格和错误列表窗格  
" **视图** " 菜单提供用于切换 "输出" 窗格和 "错误列表" 窗格的可见性的命令：  
  
-   在对象转换、对象同步和数据迁移期间，"输出" 窗格将显示来自 SSMA 的状态消息。  
  
-   "错误列表" 窗格显示可排序列表中的错误、警告和信息性消息。  
  
## <a name="see-also"></a>另请参阅  
[将 DB2 数据迁移到 SQL Server &#40;DB2ToSQL&#41;](../../ssma/db2/migrating-db2-data-into-sql-server-db2tosql.md)  
[用户界面参考 &#40;DB2ToSQL&#41;](../../ssma/db2/user-interface-reference-db2tosql.md)  
  
