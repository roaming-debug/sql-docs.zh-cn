---
title: SSMA for SAP ASE (SybaseToSQL) 入门Microsoft Docs
description: 了解 SAP ASE 安装过程的 SQL Server 迁移助手 (SSMA) ，并熟悉 SSMA 用户界面。
ms.custom: ''
ms.date: 09/30/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: c4098516-f0fc-4690-97bb-3766dfd43156
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 2ed1698af612a3bd960b7c16011dc4f95e33f61a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100078858"
---
# <a name="getting-started-with-ssma-for-sap-ase-sybasetosql"></a>SSMA for SAP ASE (SybaseToSQL) 的入门
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 利用 SAP ASE 迁移助手 (SSMA) ，可以快速将 SAP 自适应服务器企业 () ASE 转换为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 AZURE Sql 数据库架构，并将生成的架构上传到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 azure sql 数据库，以及将数据从 SAP ase 迁移到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 azure sql 数据库。  
  
本主题介绍了安装过程，并帮助你熟悉 SSMA 用户界面。  
  
## <a name="installing-and-licensing-ssma"></a>安装和授权 SSMA  
若要使用 SSMA，首先必须在可以访问 SAP ASE 的源实例和目标实例的计算机上安装 SSMA 客户端程序 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 AZURE SQL 数据库。 若要使用服务器端数据迁移，必须在运行的计算机上安装扩展包和至少一个 SAP ASE 提供程序 (OLE DB 或 ADO.NET) [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 这些组件支持数据迁移并模拟 SAP ASE 系统功能。 有关安装说明，请参阅 [安装 SSMA FOR SAP ASE &#40;SybaseToSQL&#41;](../../ssma/sybase/installing-ssma-for-sybase-sybasetosql.md)。  
  
若要启动 SSMA，请单击 "**开始**"，指向 "**所有程序**"，指向 " **[!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sybase 迁移助手**"，然后选择 " **[!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 迁移助手用于 sybase**"。 首次启动 SSMA 时，将出现 "授权" 对话框。 必须先使用 Windows Live ID 对 SSMA 进行授权，然后才能使用 SSMA。 安装说明中提供了有关安装 [SSMA For Sybase Client &#40;SybaseToSQL&#41;](../../ssma/sybase/installing-ssma-for-sybase-client-sybasetosql.md) 主题的许可说明。  
  
## <a name="ssma-for-sap-ase-user-interface"></a>SAP ASE 用户界面的 SSMA  
安装并许可 SSMA 后，可以使用 SSMA 将 SAP ASE 数据库迁移到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 AZURE SQL 数据库。 在开始之前，它有助于熟悉 SSMA 用户界面。 下图显示了 SSMA 的用户界面，包括元数据资源管理器、元数据、工具栏、输出窗格和 "错误列表" 窗格：  
  
![SAP ASE 用户界面的 SSMA](../../ssma/sybase/media/ssmaforsybaseuserinterface.jpg "SAP ASE 用户界面的 SSMA")  
  
若要开始迁移，必须首先创建一个新项目。 然后，连接到 SAP ASE。 成功连接后，将在 Sybase 元数据资源管理器中显示 SAP ASE 数据库的层次结构。 然后，你可以在 Sybase 元数据资源管理器中右键单击对象，以执行一些任务，如创建评估转换到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 AZURE SQL 数据库的报表。 还可以通过工具栏和菜单执行这些任务。  
  
还必须连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 AZURE SQL 数据库的实例。 成功连接后， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 AZURE SQL 数据库的层次结构将出现在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 SQL Azure 元数据资源管理器中。 将 SAP ASE 架构转换为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 AZURE Sql 数据库架构后，请在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 SQL Azure 元数据资源管理器中选择这些转换的架构，然后将架构加载到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 Azure SQL 数据库。  
  
将转换后的架构加载到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 AZURE Sql 数据库后，可以返回到 Sybase 元数据资源管理器，并将数据从 SAP ASE 数据库迁移到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 azure sql 数据库。  
  
有关这些任务以及如何执行这些任务的详细信息，请参阅 [将 SAP ASE 数据库迁移到 SQL Server-AZURE SQL 数据库 &#40;SybaseToSQL&#41;](../../ssma/sybase/migrating-sybase-ase-databases-to-sql-server-azure-sql-db-sybasetosql.md)。  
  
以下部分介绍 SSMA 用户界面的功能。  
  
### <a name="metadata-explorers"></a>元数据资源管理器  
SSMA 包含两个元数据资源管理器，用于浏览 SAP ASE 和 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 AZURE SQL 数据库上的操作并对其执行操作。  
  
#### <a name="sybase-metadata-explorer"></a>Sybase 元数据资源管理器  
Sybase 元数据资源管理器显示有关 SAP ASE 的源实例上的数据库的信息。  
  
通过使用 Sybase 元数据资源管理器，你可以执行以下任务：  
  
-   浏览每个数据库中的表。  
  
-   选择要转换的对象，然后将对象转换为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 AZURE SQL Database 语法。 有关详细信息，请参阅将 [SAP ASE 数据库对象转换 &#40;SybaseToSQL&#41;](../../ssma/sybase/converting-sybase-ase-database-objects-sybasetosql.md)。  
  
-   选择用于数据迁移的对象，然后将这些对象中的数据迁移到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 AZURE SQL 数据库。 有关详细信息，请参阅将 [SAP ASE 数据迁移到 SQL Server-AZURE SQL 数据库 &#40;SybaseToSQL&#41;](../../ssma/sybase/migrating-sybase-ase-data-into-sql-server-azure-sql-db-sybasetosql.md)。  
  
#### <a name="sql-server-or-sql-azure-metadata-explorer"></a>SQL Server 或 SQL Azure 元数据资源管理器  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 SQL Azure 元数据资源管理器显示有关的实例 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 AZURE SQL 数据库的信息。 当你连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 AZURE SQL 数据库的实例时，SSMA 会检索有关该实例的元数据，并将其存储在项目文件中。  
  
你可以使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 SQL Azure 元数据资源管理器选择转换后的 SAP ASE 数据库对象，然后将这些对象的 (同步) 到实例 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 Azure SQL 数据库。  
  
有关详细信息，请参阅将 [转换的数据库对象加载到 SQL Server &#40;SybaseToSQL&#41;](../../ssma/sybase/loading-converted-database-objects-into-sql-server-sybasetosql.md)。  
  
### <a name="metadata"></a>元数据  
每个元数据资源管理器右侧的选项卡都是描述所选对象的选项卡。 例如，如果您在 Sybase 元数据资源管理器中选择一个表，将出现六个选项卡： **表**、 **SQL**、 **类型映射**、 **数据**、 **属性** 和 **报表**。 仅在创建包含所选对象的报表后，" **报表** " 选项卡才包含信息。 如果在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 SQL Azure 元数据资源管理器中选择一个表，将显示以下三个选项卡： **表**、 **SQL** 和 **数据**。  
  
大多数元数据设置为只读。 但是，您可以更改以下元数据：  
  
-   在 Sybase 元数据资源管理器中，可以更改过程和类型映射。 在转换架构之前进行这些更改。  
  
-   在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 SQL Azure 元数据资源管理器中，可以更改 [!INCLUDE[tsql](../../includes/tsql-md.md)] 存储过程的。 在将架构加载到之前进行这些更改 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
在元数据资源管理器中所做的更改将反映在项目元数据中，而不是源或目标数据库中。  
  
### <a name="toolbars"></a>工具栏  
SSMA 有两个工具栏：项目工具栏和迁移工具栏。  
  
#### <a name="the-project-toolbar"></a>项目工具栏  
项目工具栏包含用于处理项目、连接到 SAP ASE 以及连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 AZURE SQL 数据库的按钮。 这些按钮类似于 " **文件** " 菜单上的命令。  
  
#### <a name="the-migration-toolbar"></a>迁移工具栏  
迁移工具栏包含以下命令：  
  
|按钮|函数|  
|----------|------------|  
|**创建报表**|将所选 SAP ASE 对象转换为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 语法，然后创建显示转换成功的报告。<br /><br />仅当在 Sybase 元数据资源管理器中选择对象时，此命令才可用。|  
|**转换架构**|将所选 SAP ASE 对象转换为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 AZURE SQL 数据库对象。<br /><br />仅当在 Sybase 元数据资源管理器中选择对象时，此命令才可用。|  
|**迁移数据**|将数据从 SAP ASE 数据库迁移到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 AZURE SQL 数据库。 在运行此命令之前，必须将 SAP ASE 架构转换为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 AZURE Sql 数据库架构，然后将对象加载到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 Azure sql 数据库。<br /><br />仅当在 Sybase 元数据资源管理器中选择对象时，此命令才可用。|  
|**Stop**|停止当前进程，如将对象转换为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 AZURE SQL Database 语法。|  
  
### <a name="menus"></a>菜单  
SSMA 包含以下菜单：  
  
|菜单|说明|  
|--------|---------------|  
|**File**|包含用于处理项目、连接到 SAP ASE 以及连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 AZURE SQL 数据库的命令。|  
|**编辑**|包含用于查找和处理详细信息页中的文本（例如 [!INCLUDE[tsql](../../includes/tsql-md.md)] 从 SQL 详细信息窗格复制）的命令。 还包含 " **管理书签** " 选项，您可以在其中查看现有书签的列表。 您可以使用对话框右侧的按钮来管理书签。|  
|**查看**|包含 **同步元数据** 资源管理器命令。 这会使 Sybase 元数据资源管理器和 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 SQL Azure 元数据资源管理器之间的对象同步。 还包含用于显示和隐藏 " **输出** " 和 " **错误列表** " 窗格以及用于管理布局的选项 **布局** 的命令。|  
|**工具**|包含用于创建报表、导出数据以及迁移对象和数据的命令。 还提供对 " **全局设置** " 和 " **项目设置** " 对话框的访问。|  
|**测试人员**|包含用于创建测试用例、查看测试结果和数据库备份管理命令的命令。|  
|**帮助**|提供对 SSMA 帮助和的 " **关于** " 对话框的访问。|  
  
### <a name="output-pane-and-error-list-pane"></a>输出窗格和错误列表窗格  
" **视图** " 菜单提供用于切换 "输出" 窗格和 "错误列表" 窗格的可见性的命令：  
  
-   在对象转换、对象同步和数据迁移期间，"输出" 窗格将显示来自 SSMA 的状态消息。  
  
-   "错误列表" 窗格在列表中显示可以排序的错误、警告和信息性消息。  
  
## <a name="see-also"></a>另请参阅  
[将 SAP ASE 数据库迁移到 SQL Server-Azure SQL 数据库 &#40;SybaseToSQL&#41;](../../ssma/sybase/migrating-sybase-ase-databases-to-sql-server-azure-sql-db-sybasetosql.md)  
[用户界面参考 &#40;SybaseToSQL&#41;](../../ssma/sybase/user-interface-reference-sybasetosql.md)  
