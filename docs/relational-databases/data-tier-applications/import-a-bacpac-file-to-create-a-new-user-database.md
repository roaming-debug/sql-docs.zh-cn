---
description: 导入 BACPAC 文件以创建新的用户数据库
title: 导入 BACPAC 文件以创建新的用户数据库
ms.date: 06/03/2020
ms.prod: sql
ms.technology: ''
ms.topic: conceptual
f1_keywords:
- sql13.swb. importdac.results.f1
- sql13.swb.importdac.settings.f1
- sql13.swb.importdac.storagebrowser.f1
- sql13.swb.importdac.results.f1
- sql13.swb.importdac.progress.f1
- sql13.swb. importdac.summary.f1
- sql13.swb.importdac.summary.f1
- sql13.swb. importdac.progress.f1
- sql13.swb.importdac.welcome.f1
- sql13.swb. importdac.settings.f1
helpviewer_keywords:
- Data-tier application
- SQL Server DAC
- Migrate database
- DAC
ms.assetid: 736d8d9a-39f1-4bf8-b81f-2e56c134d12e
author: stevestein
ms.author: sstein
ms.custom: seo-dt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 77209c7b5156956897c3c458e825fddeb7462e22
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "92195579"
---
# <a name="import-a-bacpac-file-to-create-a-new-user-database"></a>导入 BACPAC 文件以创建新的用户数据库
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  导入数据层应用程序 (DAC) 文件 - .bacpac 文件 - 以在 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 的新实例上创建一个带数据的原始数据库的副本，或者对 [!INCLUDE[ssSDSFull](../../includes/sssdsfull-md.md)] 创建一个此数据库的副本。 可以将导出-导入操作结合起来在各实例之间迁移 DAC 或数据库，或者创建一个逻辑备份，例如创建部署在 [!INCLUDE[ssSDS](../../includes/sssds-md.md)] 中的数据库的本地副本。  
  
## <a name="before-you-begin"></a>开始之前  
 导入过程分两个阶段生成新的 DAC。  
  
1.  导入过程使用存储在导出文件中的 DAC 定义创建新的 DAC 和关联的数据库，同样，DAC 部署使用 DAC 包文件中的定义创建新的 DAC。  
  
2.  导入过程将从导出文件中大容量复制数据。  

## <a name="sql-server-utility"></a>SQL Server 实用工具  
 如果将 DAC 导入到数据库引擎的实例，则在下次将实用工具收集组从该实例发送到实用工具控制点时，导入的 DAC 将合并到 SQL Server 实用工具中。 然后，该 DAC 将出现在 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] 实用工具资源管理器的“已部署的数据层应用程序”节点中，并且将在“已部署的数据层应用程序”的详细信息页面中报告  。  
  
## <a name="database-options-and-settings"></a>数据库选项和设置  
 默认情况下，在导入过程中创建的数据库将具有来自 CREATE DATABASE 语句的几乎所有默认设置，而例外的是数据库排序规则和兼容级别设置为在 DAC 导出文件中定义的值。 DAC 导出文件将使用来自原始数据库的值。  
  
 某些数据库选项（例如 TRUSTWORTHY、DB_CHAINING 和 HONOR_BROKER_PRIORITY）不能作为导入过程的一部分进行调整。 物理属性（如文件组的数目或文件的数目和大小）不能作为导入过程的一部分进行更改。 在导入完成后，可以使用 ALTER DATABASE 语句、 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]或 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] PowerShell 对数据库进行定制。 有关详细信息，请参阅 [Databases](../../relational-databases/databases/databases.md)。  
  
## <a name="limitations-and-restrictions"></a>限制和局限  
 可以将 DAC 导入到 [!INCLUDE[ssSDS](../../includes/sssds-md.md)]或运行 [!INCLUDE[ssDE](../../includes/ssde-md.md)] Service Pack 4 (SP4) 或更高版本的 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 实例。 如果从更高版本中导出 DAC，则 DAC 可能包含 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]不支持的对象。 您不能将这些 DAC 部署到 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]实例。  
  
## <a name="prerequisites"></a>先决条件  
 建议您不要从未知或不可信的源导入 DAC 导出文件。 此类文件可能包含恶意代码，这些代码可能会执行非预期的 Transact-SQL 代码，或者通过修改架构导致错误。 在使用来自未知或不可信源的导出文件之前，请解压缩该 DAC 并检查代码，例如存储过程或者其他用户定义的代码。 有关如何执行这些检查的详细信息，请参阅 [Validate a DAC Package](validate-a-dac-package.md)。  
  
## <a name="security"></a>安全性  
 为了提高安全性，SQL Server 身份验证登录名存储在 DAC 导出文件中且没有密码。 在导入该文件时，登录名将作为含有生成的密码的已禁用登录名创建。 若要启用这些登录名，请使用具有 ALTER ANY LOGIN 权限的登录名登录，并且使用 ALTER LOGIN 来启用该登录名和分配可以传达给用户的新密码。 对于 Windows 身份验证登录名则无需执行此操作，因为其密码不是由 SQL Server 管理的。  
  
## <a name="permissions"></a>权限  
 DAC 只能由 **sysadmin** 或 **serveradmin** 固定服务器角色的成员导入，或者由具有 **dbcreator** 固定服务器角色且具有 ALTER ANY LOGIN 权限的登录名导入。 名为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sa **的内置** 系统管理员帐户也可以导入 DAC。 将具有登录名的 DAC 导入到 [!INCLUDE[ssSDS](../../includes/sssds-md.md)] 要求 loginmanager 或 serveradmin 角色的成员身份。 将不具有登录名的 DAC 导入到 [!INCLUDE[ssSDS](../../includes/sssds-md.md)] 要求 dbmanager 或 serveradmin 角色的成员身份。  
  
## <a name="using-the-import-data-tier-application-wizard"></a>使用“导入数据层应用程序向导”  
 **若要启动向导，则使用以下步骤：**  
  
1.  连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的实例（无论是在内部部署中还是在 [!INCLUDE[ssSDS](../../includes/sssds-md.md)]中）。  
  
2.  在“对象资源管理器” 中，右键单击“数据库” ，然后选择“导入数据层应用程序”  菜单项以启动向导。  
  
3.  完成向导对话框：  
  
    -   [“简介”页](#Introduction)  
  
    -   [“导入设置”页](#Import_settings)  
  
    -   [“数据库设置”页](#Database_settings)  
  
    -   [摘要页](#Summary)  
  
    -   [“进度”页](#Progress)  
  
    -   [“结果”页](#Results)  
  
###  <a name="introduction-page"></a><a name="Introduction"></a> “简介”页  
 此页介绍“数据层应用程序导入向导”的各个步骤。  
  
 **选项**  
  
-   **不再显示此页。** - 选中此复选框可以停止在将来显示“简介”页。  
  
-   **下一步** - 进入“导入设置”页。  
  
-   **取消** - 取消操作并关闭向导。  
  
###  <a name="import-settings-page"></a><a name="Import_settings"></a> “导入设置”页  
 使用此页可以指定要导入的 .bacpac 文件的位置。  
  
-   **从本地磁盘导入** - 单击“浏览…”以导航本地计算机，或在提供的空间中指定路径。 路径名必须包含文件名和 .bacpac 扩展名。  
  
-   **从 Azure 导入** - 从 Microsoft Azure 容器中导入一个 BACPAC 文件。 必须连接到 Microsoft Azure 容器才能验证此选项。 请注意，“从 Azure 导入”选项还要求为临时文件指定一个本地目录。 将在指定位置创建临时文件，并且在操作完成后，临时文件将保留在该位置。  
  
     在浏览 Azure 时，你将能够在单一帐户内的不同容器之间进行切换。 您必须指定一个单独的 .bacpac 文件以继续导入操作。 可以按“名称”、“大小”或“修改日期”对列进行排序。  
  
     若要继续，请指定要导入的 .bacpac 文件，然后单击 **“打开”**。  
  
###  <a name="database-settings-page"></a><a name="Database_settings"></a> “数据库设置”页  
 使用此页面可以指定将要创建的数据库的详细信息。  
  
 **对于 SQL Server 的本地实例：**  
  
-   **新数据库名称** - 提供导入的数据库的名称。  
  
-   **数据文件路径** - 提供数据文件的本地目录。 单击“浏览…”以导航本地计算机，或在提供的空间中指定路径  。  
  
-   **日志文件路径** - 提供日志文件的本地目录。 单击“浏览…”以导航本地计算机，或在提供的空间中指定路径  。  
  
 若要继续，请单击 **“下一步”** 。  
  
 **对于 Azure SQL 数据库：**  
  
 - [导入 BACPAC 文件以创建新的 Azure SQL 数据库](/azure/azure-sql/database/database-import) 提供了有关使用 Azure 门户、PowerShell、SSMS 或 SqlPackage 的分步说明。  
 - 有关不同服务层的详细信息，请查阅 **[SQL 数据库选项和性能：了解每个服务层中的可用功能](/azure/azure-sql/database/purchasing-models)** 。  

### <a name="validation-page"></a>“验证”页  
 使用此页可查看阻止操作的任何问题。 若要继续，请解决阻止问题，然后单击“重新运行验证”  确保验证成功。  
  
 若要继续，请单击 **“下一步”** 。  
  
###  <a name="summary-page"></a><a name="Summary"></a> 摘要页  
 使用此页可查看操作的指定的源和目标设置。 若要使用指定设置完成导入操作，请单击 **“完成”**。 若要取消导入操作并退出向导，请单击“取消” 。  
  
###  <a name="progress-page"></a><a name="Progress"></a> “进度”页  
 此页将显示一个指示操作状态的进度栏。 若要查看详细状态，请单击 **“查看详细信息”** 选项。  
  
 若要继续，请单击 **“下一步”** 。  
  
###  <a name="results-page"></a><a name="Results"></a> “结果”页  
 此页将报告导入和创建数据库操作是成功还是失败，并显示各个操作的成功或失败。 遇到了错误的任何操作都将在 **“结果”** 列中具有一个链接。 单击该链接可以查看针对该操作的错误报告。  
  
 单击“**关闭**”以关闭向导。  
  
## <a name="see-also"></a>另请参阅  
[导入 BACPAC 文件以创建新的 Azure SQL 数据库](/azure/azure-sql/database/database-import)  
 [数据层应用程序](../../relational-databases/data-tier-applications/data-tier-applications.md)   
 [导出数据层应用程序](../../relational-databases/data-tier-applications/export-a-data-tier-application.md)  
  
