---
description: " (AccessToSQL) 创建和管理项目"
title: " (AccessToSQL) 创建和管理项目 |Microsoft Docs"
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- creating projects
- new projects
- opening projects
- projects
- projects, creating and managing
- saving metadata
- saving projects
ms.assetid: f2d1f0b0-5394-4adb-b3f3-abd71eb68ca7
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: ba2e567c5675e602cc80382d42adfcbb89bce90a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100045007"
---
# <a name="creating-and-managing-projects-accesstosql"></a> (AccessToSQL) 创建和管理项目
若要将 Access 数据库迁移到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 SQL Azure，必须先创建 SSMA 项目。 项目是一个文件，其中包含有关要迁移到或 SQL Azure 的数据库的元数据 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或将接收迁移对象和数据的 SQL Azure 的元数据、 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 连接信息和项目设置。  
  
## <a name="reviewing-default-project-settings"></a>查看默认项目设置  
SSMA 包含用于转换和同步数据库对象和转换数据的几个选项。 这些选项的默认设置适用于许多用户。 但是，在创建新的 SSMA 项目之前，应该查看选项，如果需要，请更改将用于所有新项目的默认设置。  
  
**查看默认项目设置**  
  
1.  在 " **工具** " 菜单上，选择 " **默认项目设置**"。  
  
2.  选择要查看/更改其设置的 " **迁移目标版本** " 下拉的项目类型，然后单击 " **常规** " 选项卡。  
  
3.  在左窗格中，单击 " **转换**"。  
  
4.  在右侧窗格中，查看选项。 有关这些选项的详细信息，请参阅 [项目设置 (转换) ](./project-settings-conversion-accesstosql.md)。  
  
5.  根据需要更改选项。  
  
6.  为 " **迁移**"、" **GUI**" 和 " **类型" 映射** 页重复前面的步骤。  
  
    -   有关迁移选项的信息，请参阅 [项目设置 (迁移) ](./project-settings-migration-accesstosql.md)。  
  
    -   有关用户界面选项的信息，请参阅 [项目设置 (GUI) ](../sybase/project-settings-gui-sybasetosql.md)。  
  
    -   有关数据类型映射设置的详细信息，请参阅 [项目设置 (类型映射) ](./project-settings-type-mapping-accesstosql.md)。  
  
    -   有关 SQL Azure 设置的信息，请参阅 [项目设置 (SQL Azure) ](./project-settings-azure-sql-db-accesstosql.md)。  
  
**注意** 仅当在创建项目时选择要 SQL Azure 的迁移时，SQL Azure 设置才可用。  
  
## <a name="creating-new-projects"></a>创建新项目  
SSMA 在不加载默认项目的情况下启动。 若要将数据从 Access 数据库迁移到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 SQL Azure，必须创建一个项目。  
  
**创建新项目**  
  
1.  在“文件”菜单中，选择“新建项目”。  
  
    将显示“新建项目”对话框。  
  
2.  在 " **名称** " 框中，输入项目的名称。  
  
3.  在 " **位置** " 框中，输入或选择项目的文件夹  
  
4.  在 "迁移到" 下拉 SQL Server 2005/SQL Server 2008/SQL Server 2012/SQL Server 2014/SQL Server 2016/Azure SQL 数据库中选择一个，然后单击 **"确定"**。  
  
SSMA 创建项目文件。 你现在可以执行 [添加一个或多个 Access 数据库](adding-and-removing-access-database-files-accesstosql.md)的下一步。  
  
## <a name="customizing-project-settings"></a>自定义项目设置  
除了定义适用于所有新 SSMA 项目的默认项目设置，你还可以自定义每个项目的设置。 有关详细信息，请参阅 [设置转换和迁移选项](setting-conversion-and-migration-options-accesstosql.md)。  
  
在源数据库和目标数据库之间自定义数据类型映射时，可以在项目、数据库或对象级别定义映射。 有关类型映射的详细信息，请参阅 [映射源和目标数据类型](mapping-source-and-target-data-types-accesstosql.md)。  
  
## <a name="saving-projects"></a>保存项目  
保存项目时，SSMA 会将项目设置（可选）保存到项目文件。  
  
**保存项目的步骤**  
  
-   在 " **文件** " 菜单上，选择 " **保存项目**"。  
  
    如果项目中的数据库已更改或尚未转换，SSMA 会提示你将元数据保存到项目中。 保存元数据使你可以脱机工作。 它还允许您将一个完整的项目文件发送给其他人，包括技术支持人员。 如果系统提示你保存元数据，请执行以下操作：  
  
    1.  对于显示 " **缺少元数据**" 状态的每个数据库，请选中数据库名称旁边的复选框。  
  
        保存元数据可能需要几分钟时间。 如果此时不想保存元数据，请不要选中任何复选框。  
  
    2.  单击“ **保存**”。  
  
        SSMA 将分析访问架构，并将元数据保存到项目文件。  
  
## <a name="opening-projects"></a>打开项目  
打开某个项目时，它会断开与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 SQL Azure 的连接。 这使你可以脱机工作。 若要更新元数据，请将数据库对象加载到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 SQL Azure。 若要迁移数据，必须重新连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 SQL Azure。  
  
**打开项目的步骤**  
  
1.  可使用下列过程之一：  
  
    -   在 " **文件** " 菜单上，指向 " **最近使用的项目**"，然后选择要打开的项目。  
  
    -   在 " **文件** " 菜单上，选择 " **打开项目**"，找到 "a2ssproj" 项目文件，选择该文件，然后单击 " **打开**"。  
  
2.  若要重新连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，请在 " **文件** " 菜单上，选择 " **重新连接到 SQL Server**"。  
  
3.  若要重新连接到 SQL Azure，请在 " **文件** " 菜单上，选择 " **重新连接到 SQL Azure"。**  
  
## <a name="next-step"></a>下一步  
迁移过程的下一步是 [添加一个或多个访问数据库](adding-and-removing-access-database-files-accesstosql.md)。  
  
## <a name="see-also"></a>另请参阅  
[将 Access 数据库迁移到 SQL Server](migrating-access-databases-to-sql-server-azure-sql-db-accesstosql.md)  
[添加和删除 Access 数据库文件](adding-and-removing-access-database-files-accesstosql.md)  
