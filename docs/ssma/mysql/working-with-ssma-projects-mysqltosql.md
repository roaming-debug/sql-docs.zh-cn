---
description: 处理 SSMA 项目 (MySQLToSQL)
title: 使用 SSMA 项目 (MySQLToSQL) |Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Working with SSMA projects, create new project
- Working with SSMA projects, customize settings
- Working with SSMA projects, Open project
- Working with SSMA projects, Save project
ms.assetid: 9e4394e9-f177-41d9-839e-5d53a9c9b840
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 92fb1bec694ca29ffe0591eb03012e1ed2ab3f2f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100064362"
---
# <a name="working-with-ssma-projects-mysqltosql"></a>处理 SSMA 项目 (MySQLToSQL)
若要将 MySQL 数据库迁移到 SQL Server 或 SQL Azure，必须先创建 SSMA 项目。 项目是包含以下信息的文件：  
  
-   要迁移到 SQL Server 或 SQL Azure 的 MySQL 数据库的相关元数据。  
  
-   有关将接收迁移的对象和数据的 SQL Server 或 SQL Azure 的目标实例的元数据。  
  
-   SQL Server 或 SQL Azure 连接信息。  
  
-   项目设置。  
  
打开某个项目时，它会与 MySQL 断开连接，并 SQL Server 或 SQL Azure。 这使你可以脱机工作。 有关重新连接到 SQL Server 的详细信息，请参阅 [连接到 SQL Server &#40;MySQLToSQL&#41;](../../ssma/mysql/connecting-to-sql-server-mysqltosql.md)  
  
## <a name="reviewing-default-project-settings"></a>查看默认项目设置  
SSMA 包含一些设置，用于转换和加载数据库、迁移数据以及将 SSMA 与 MySQL 和 SQL Server 或 SQL Azure 同步。 默认设置适用于许多用户。 但是，在创建新的 SSMA 项目之前，应该检查设置。 如果需要，你可以更改将用于所有新项目的默认设置。  
  
##### <a name="to-review-default-project-settings"></a>查看默认项目设置  
  
1.  从 "**工具**" 菜单中选择 "**默认项目设置**"。  
  
2.  选择要查看/更改其设置的 " **迁移目标版本** " 下拉的项目类型，然后单击 " **常规** " 选项卡。  
  
3.  在左窗格中，单击 " **转换**"。  
  
4.  在右侧窗格中，根据需要查看和更改设置。 有关这些设置的详细信息，请参阅 [MySQLToSQL&#41;&#40;转换&#41; &#40;项目设置 ](../../ssma/mysql/project-settings-conversion-mysqltosql.md) 。  
  
5.  对于 "迁移"、"同步"、"SQL Azure"、"GUI" 和 "类型映射" 页，重复步骤1-3。  
  
-   有关迁移设置的信息，请参阅 [项目设置 &#40;迁移&#41; &#40;MySQLToSQL&#41;](../../ssma/mysql/project-settings-migration-mysqltosql.md)。  
  
-   有关同步到 SQL Server 的设置的信息，请参阅 [项目设置 &#40;同步&#41; &#40;MySQLToSQL&#41;](../../ssma/mysql/project-settings-synchronization-mysqltosql.md)。  
  
-   有关 GUI 设置的信息，请参阅 [ (GUI 的项目设置)  (SSMA Common) ](../sybase/project-settings-gui-sybasetosql.md)。  
  
-   有关数据类型映射设置的信息，请参阅 [项目设置 &#40;类型映射&#41; &#40;MySQLToSQL&#41;](../../ssma/mysql/project-settings-type-mapping-mysqltosql.md)。  
  
-   有关 SQL Azure 设置的信息，请参阅 [AZURE SQL 数据库的项目设置 &#40;&#41; &#40;MySQLToSQL&#41;](../../ssma/mysql/project-settings-azure-sql-db-mysqltosql.md)。  
  
> [!NOTE]  
> 只有当你在创建项目时选择 " **迁移到 SQL Azure** " 时，才会显示 SQL Azure 设置。  
  
## <a name="creating-new-projects"></a>创建新项目  
若要将数据从 MySQL 数据库迁移到 SQL Server 或 SQL Azure，您必须创建一个项目。  
  
##### <a name="to-create-a-new-project"></a>创建新项目的步骤  
  
1.  从 "**文件**" 菜单中选择 "**新建项目**"。 将显示“新建项目”对话框。 在“文件”菜单中，选择“新建项目”。 将显示“新建项目”对话框。  
  
2.  在 " **名称** " 框中，输入项目的名称。  
  
3.  在 " **位置** " 框中，为项目输入或选择一个文件夹。  
  
4.  在 " **迁移到** " 下拉菜单中，选择用于迁移的目标版本 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 可用选项包括：  
  
    -   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2005  
  
    -   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2008  
  
    -   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2012  
  
    -   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2014  
  
    -   Azure SQL 数据库  
  
然后单击 **"确定"**  
  
SSMA 创建项目文件。  
  
## <a name="customizing-project-settings"></a>自定义项目设置  
除了定义适用于所有新 SSMA 项目的默认项目设置，还可以自定义每个项目的设置。 有关详细信息，请参阅 [设置项目选项 &#40;MySQLToSQL&#41;](../../ssma/mysql/setting-project-options-mysqltosql.md)。  
  
在源数据库和目标数据库之间自定义数据类型映射时，可以在项目、数据库或对象级别定义映射。 有关详细信息，请参阅 [映射 MySQL 和 SQL Server 数据类型 &#40;MySQLToSQL&#41;](../../ssma/mysql/mapping-mysql-and-sql-server-data-types-mysqltosql.md)。  
  
## <a name="saving-projects"></a>保存项目  
通过 "保存项目" 功能，用户可以在本质上保存项目设置，还可以选择将数据库元数据保存到 SSMA 项目文件。  
  
##### <a name="to-save-a-project"></a>保存项目的步骤  
  
-   在 " **文件** " 菜单上，选择 " **保存** 项目"。  
  
如果项目中的数据库已更改或尚未转换，SSMA 会提示你加载并保存元数据。 加载和保存元数据使你可以脱机工作。 它还允许你向其他人（如技术支持人员）发送完整的项目文件。 如果系统提示你保存元数据，请执行以下操作：  
  
1.  对于显示 " **缺少元数据**" 状态的每个数据库，请选中数据库名称旁边的复选框。 保存元数据可能需要几分钟时间。 如果此时不想保存元数据，请不要选中任何复选框。  
  
2.  单击“ **保存**”。  
  
SSMA 将分析 MySQL 架构，并将元数据保存到项目文件。  
  
## <a name="opening-projects"></a>打开项目  
打开某个项目时，它会与 MySQL 断开连接，也不会从 SQL Server 或 SQL Azure 断开连接。 这使你可以脱机工作。 若要更新元数据，请将数据库对象加载到 SQL Server 或 SQL Azure 中。 若要迁移数据，必须重新连接到 SQL Server 或 SQL Azure。  
  
##### <a name="to-open-a-project"></a>打开项目的步骤  
  
1.  可使用下列过程之一：  
  
    1.  在 " **文件** " 菜单上，指向 " **最近使用的项目**"。  
  
    2.  选择要打开的项目。  
  
    3.  在 " **文件** " 菜单上，选择 " **打开项目**"，找到 "m2ssproj" 项目文件，选择该文件，然后单击 " **打开**"。  
  
2.  若要重新连接到 MySQL，请在 " **文件** " 菜单上，选择 " **重新连接到 mysql**"。  
  
3.  若要重新连接到 SQL Server，请在 " **文件** " 菜单上，选择 " **重新连接到 SQL Server**"。  
  
4.  若要重新连接到 SQL Azure，请在 " **文件** " 菜单上，选择 " **重新连接到 SQL Azure"。**  
  
## <a name="next-step"></a>下一步  
迁移过程的下一步是 [连接到 MySQL &#40;MySQLToSQL&#41;](../../ssma/mysql/connecting-to-mysql-mysqltosql.md)  
  
## <a name="see-also"></a>另请参阅  
[连接到 MySQL &#40;MySQLToSQL&#41;](../../ssma/mysql/connecting-to-mysql-mysqltosql.md)  
[将 MySQL 数据库迁移到 SQL Server-Azure SQL 数据库 &#40;MySQLToSql&#41;](../../ssma/mysql/migrating-mysql-databases-to-sql-server-azure-sql-db-mysqltosql.md)  
[连接到 SQL Server &#40;MySQLToSQL&#41;](../../ssma/mysql/connecting-to-sql-server-mysqltosql.md)  
[连接到 Azure SQL 数据库 &#40;MySQLToSQL&#41;](../../ssma/mysql/connecting-to-azure-sql-db-mysqltosql.md)  
