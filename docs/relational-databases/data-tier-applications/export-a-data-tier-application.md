---
description: 导出数据层应用程序
title: 导出数据层应用程序 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.technology: ''
ms.topic: conceptual
f1_keywords:
- sql13.swb.exportdac.progress.f1
- sql13.swb.exportdac.summary.f1
- sql13.swb.exportdac.results.f1
- sql13.swb. exportdac.results.f1
- sql13.swb. exportdac.summary.f1
- sql13.swb. exportdac.settings.f1
- sql13.swb.exportdac.welcome.f1
- sql13.swb.exportdac.settings.f1
helpviewer_keywords:
- How to [DAC], export
- wizard [DAC], export
- export DAC
- data-tier application [SQL Server], export
ms.assetid: 61915bc5-0f5f-45ac-8cfe-3452bc185558
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 2e4ad904176abdc4f714980cf1e242bd620632a6
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97440211"
---
# <a name="export-a-data-tier-application"></a>导出数据层应用程序
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  导出部署的数据层应用程序 (DAC) 或数据库将创建一个导出文件，其中同时包括该数据库中的对象定义和表中包含的所有数据。 然后，可将该导出文件导入到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]的其他实例或 [!INCLUDE[ssSDSFull](../../includes/sssdsfull-md.md)]中。 可以将导出-导入操作组合起来以在实例之间迁移 DAC，或者创建一个存档，或者创建部署在 [!INCLUDE[ssSDS](../../includes/sssds-md.md)]中的数据库的本地副本。  
  
## <a name="before-you-begin"></a>开始之前  
 导出过程分两个阶段生成一个 DAC 导出文件。  
  
1.  导出操作在导出文件中生成 DAC 定义（BACPAC 文件），DAC 提取操作以同样的方式在 DAC 包文件中生成 DAC 定义。 导出的 DAC 定义包含当前数据库中的所有对象。 如果针对最初从 DAC 部署的数据库运行导出过程，且在部署后对数据库直接进行了更改，则导出的定义与数据库中的对象集匹配，而不是与原始 DAC 中定义的内容相匹配。  
  
2.  导出操作从数据库中的所有表中大容量复制数据，然后将这些数据集成到导出文件中。  

 导出过程将 DAC 版本设置为 1.0.0.0，并将导出文件中的 DAC 说明设置为空字符串。 如果从 DAC 部署数据库，则导出文件中的 DAC 定义包含为原始 DAC 指定的名称，否则，将把 DAC 名称设置为数据库名称。  
  

###  <a name="limitations-and-restrictions"></a><a name="LimitationsRestrictions"></a> 限制和局限  
 只能从 [!INCLUDE[ssSDS](../../includes/sssds-md.md)]或 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] Service Pack 4 (SP4) 或更高版本的数据库导出 DAC 或数据库。  
  
 如果数据库有 DAC 中不支持的对象或包含用户，则不能导出该数据库。 有关 DAC 中支持的对象类型的详细信息，请参阅 [DAC Support For SQL Server Objects and Versions](/previous-versions/sql/sql-server-2012/ee210549(v=sql.110))。  
  
###  <a name="permissions"></a><a name="Permissions"></a> 权限  
 导出 DAC 至少要求 ALTER ANY LOGIN 和数据库范围内的 VIEW DEFINITION 权限，以及对 **sys.sql_expression_dependencies** 具有 SELECT 权限。 导出 DAC 可由 securityadmin 固定服务器角色的成员（也是从其导出 DAC 的数据库中 database_owner 固定数据库角色的成员）完成。 sysadmin 固定服务器角色的成员或名为 **sa** 的内置 SQL Server 系统管理员帐户也可以导出 DAC。
 
在 Azure SQL 数据库上，需要为每个数据库授予对所有表或特定表的“查看定义”和“选择”权限

  
##  <a name="using-the-export-data-tier-application-wizard"></a><a name="UsingDeployDACWizard"></a> 使用“导出数据层应用程序向导”  
 **使用向导导出 DAC**  
  
1.  连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的实例（无论是在内部部署中还是在 [!INCLUDE[ssSDS](../../includes/sssds-md.md)]中）。  
  
2.  在 **“对象资源管理器”** 中，展开要从中导出 DAC 的实例的节点。  
  
3.  右键单击数据库名称。  
  
4.  单击“任务”，然后选择“导出数据层应用程序…”    
  
5.  完成向导对话框：  
  
    -   [“简介”页](#Introduction)  
  
    -   [“导出设置”页](#Export_settings)  
  
    -   [“验证”页](#Validation)  
  
    -   [摘要页](#Summary)  
  
    -   [“进度”页](#Progress)  
  
    -   [“结果”页](#Results)  
  
##  <a name="introduction-page"></a><a name="Introduction"></a> “简介”页  
 此页介绍“导出数据层应用程序向导”的各个步骤。  
  
 **选项**  
  
 **不再显示此页。** - 选中此复选框可以停止在将来显示“简介”页。  
  
 **下一步** - 继续到“选择 DAC 包”  页。  
  
 **取消** - 取消操作并关闭向导。  
  
##  <a name="export-settings-page"></a><a name="Export_settings"></a> “导出设置”页  
 使用此页可以指定要创建 BACPAC 文件的位置。  
  
-   **保存到本地磁盘** - 在本地计算机上的目录中创建 BACPAC 文件。 单击“浏览…”以导航本地计算机，或在提供的空间中指定路径  。 路径名必须包含文件名和 .bacpac 扩展名。  
  
-   **保存到 Azure** - 在 Azure 容器中创建 BACPAC 文件。 若要验证此选项，则必须连接到 Azure 容器。 请注意，此选项还要求您为临时文件指定一个本地目录。 请注意，将在指定位置创建临时文件，并且在操作完成后，临时文件将保留在该位置。  
  
 若要指定要导出的表的子集，请使用 **“高级”** 选项。  
  
##  <a name="validation-page"></a><a name="Validation"></a> “验证”页  
 使用验证页可查看阻止操作的任何问题。 若要继续，请解决阻止问题，然后单击“重新运行验证”  确保验证成功。  
  
 若要继续，请单击 **“下一步”** 。  
  
##  <a name="summary-page"></a><a name="Summary"></a> 摘要页  
 使用此页可查看操作的指定的源和目标设置。 若要使用指定设置完成导出操作，请单击 **“完成”** 。 若要取消导出操作并退出向导，请单击 **“取消”** 。  
  
##  <a name="progress-page"></a><a name="Progress"></a> “进度”页  
 此页将显示一个指示操作状态的进度栏。 若要查看详细状态，请单击 **“查看详细信息”** 选项。  
  
##  <a name="results-page"></a><a name="Results"></a> “结果”页  
 此页将报告导出操作是成功还是失败，并显示每个操作的结果。 遇到了错误的任何操作都将在 **“结果”** 列中具有一个链接。 单击该链接可以查看针对该操作的错误报告。  
  
 单击 **“完成”** 关闭向导。  
  
##  <a name="using-a-net-framework-application"></a><a name="NetApp"></a> 使用 .Net Framework 应用程序  
 **使用 .Net Framework 应用程序中的 Export() 方法导出 DAC。**  
  
 若要查看代码示例，请下载有关 [Codeplex](https://go.microsoft.com/fwlink/?LinkId=219575)的 DAC 示例应用程序  
  
1.  创建一个 SMO Server 对象，并且将该对象设置为包含要导出的 DAC 的实例。  
  
2.  打开 **ServerConnection** 对象，并连接到同一实例。  
  
3.  使用 **Export** 类型的 **Microsoft.SqlServer.Management.Dac.DacStore** 方法导出 DAC。 指定要导出的 DAC 的名称以及指向将用于放置导出文件的文件夹的路径。  
  
## <a name="see-also"></a>另请参阅  
 [数据层应用程序](../../relational-databases/data-tier-applications/data-tier-applications.md)   
 [从数据库中提取 DAC](../../relational-databases/data-tier-applications/extract-a-dac-from-a-database.md)  
  
