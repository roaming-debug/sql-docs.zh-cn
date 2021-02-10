---
description: '使用 SSMA 项目 (DB2ToSQL) '
title: 使用 SSMA 项目 (DB2ToSQL) |Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 07abef8a-28e8-4a66-927c-c9a5b8c938ef
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 67c772195ea4b2a2ab29c08219f77d04a5d6833c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100069302"
---
# <a name="working-with-ssma-projects-db2tosql"></a>使用 SSMA 项目 (DB2ToSQL) 
若要将 DB2 数据库迁移到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，请先创建一个 SSMA 项目。 项目是包含以下信息的文件：  
  
-   有关要迁移到的 DB2 数据库的元数据 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]将接收已迁移对象和数据的的目标实例的元数据。  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 连接信息。  
  
-   项目设置。  
  
当你打开项目时，它会与 DB2 和断开连接 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 这使你可以脱机工作。 有关重新连接到的信息 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，请参阅 [连接到 SQL Server &#40;DB2ToSQL&#41;](../../ssma/db2/connecting-to-sql-server-db2tosql.md)。  
  
## <a name="reviewing-default-project-settings"></a>查看默认项目设置  
SSMA 包含多个用于转换和加载数据库对象、迁移数据以及将 SSMA 与 DB2 和同步的设置 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 默认设置适用于许多用户。 但是，在创建新的 SSMA 项目之前，应该检查设置。 如果需要，你可以更改将用于所有新项目的默认设置。  
  
**查看默认项目设置**  
  
1.  在 " **工具** " 菜单上，单击 " **默认项目设置**"。  
  
2.  在 " **迁移目标版本** " 下拉顺序中选择需要查看或更改其设置的项目类型，然后单击 " **常规** " 选项卡。  
  
3.  在左窗格中，单击 " **转换**"。  
  
4.  在右侧窗格中，根据需要查看和更改设置。 有关这些设置的详细信息，请参阅 [DB2ToSQL&#41;&#40;转换&#41; &#40;项目设置 ](../../ssma/db2/project-settings-conversion-db2tosql.md)。  
  
5.  对于迁移、同步、加载系统对象、GUI 和类型映射页，重复步骤1-3。  
  
    -   有关迁移设置的信息，请参阅 [项目设置 &#40;迁移&#41; &#40;DB2ToSQL&#41;](../../ssma/db2/project-settings-migration-db2tosql.md)。  
  
    -   有关系统对象设置的信息，请参阅 [&#40;&#41; &#40;DB2ToSQL&#41;加载系统对象的项目设置 ](../../ssma/db2/project-settings-loading-system-objects-db2tosql.md)。  
  
    -   有关同步的设置的信息 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，请参阅 [项目设置&#40;同步&#41; &#40;DB2ToSQL&#41;](../../ssma/db2/project-settings-synchronization-db2tosql.md)。  
  
    -   有关 GUI 设置的信息，请参阅 [项目设置 &#40;GUI&#41; &#40;DB2ToSQL&#41;](../../ssma/db2/project-settings-gui-db2tosql.md)。  
  
    -   有关数据类型映射设置的信息，请参阅 [项目设置 &#40;类型映射&#41; &#40;DB2ToSQL&#41;](../../ssma/db2/project-settings-type-mapping-db2tosql.md)。  
  
## <a name="creating-new-projects"></a>创建新项目  
若要将数据从 DB2 数据库迁移到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，你必须先创建一个项目。  
  
**创建项目**  
  
1.  在“文件”菜单上，单击“新建项目”。  
  
    将显示“新建项目”对话框。  
  
2.  在 " **名称** " 框中，输入项目的名称。  
  
3.  在 " **位置** " 框中，输入或选择项目的文件夹，然后单击 **"确定"**。  
  
4.  在 " **迁移到** " 下拉菜单中，选择用于迁移的目标版本 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 可用选项包括：  
  
    -   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2012  
  
    -   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2014  
  
    -   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2016  
  
    -   Azure SQL 数据库  
  
## <a name="customizing-project-settings"></a>自定义项目设置  
除了定义适用于所有新 SSMA 项目的默认项目设置，你还可以自定义每个项目的设置。 有关详细信息，请参阅 [设置项目选项 &#40;OracleToSQL&#41;](../../ssma/oracle/setting-project-options-oracletosql.md) 和相关部分。  
  
在源数据库和目标数据库之间自定义数据类型映射时，可以在项目、数据库或对象级别定义映射。 有关详细信息，请参阅 [映射 DB2 和 SQL Server 数据类型 &#40;DB2ToSQL&#41;](../../ssma/db2/mapping-db2-and-sql-server-data-types-db2tosql.md)。  
  
## <a name="saving-projects"></a>保存项目  
保存项目时，SSMA 会将项目设置和数据库元数据保留到项目文件中。  
  
**保存项目的步骤**  
  
-   在 " **文件** " 菜单上，单击 " **保存项目**"。  
  
    如果项目中的架构已更改或尚未转换，SSMA 会提示你加载并保存元数据。 加载和保存元数据将允许脱机工作。 它还允许你向其他人（如技术支持人员）发送完整的项目文件。 如果系统提示你保存元数据，请执行以下操作：  
  
    1.  对于显示 " **缺少元数据**" 状态的每个架构，请选中数据库名称旁边的复选框。  
  
        保存元数据可能需要几分钟时间。 如果你不想保存元数据，请不要选中任何复选框。  
  
    2.  单击“保存”按钮  。  
  
        SSMA 将分析 DB2 架构，并将元数据保存到项目文件。  
  
## <a name="opening-projects"></a>打开项目  
打开某个项目时，该项目将从 DB2 和中断开连接 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 这使你可以脱机工作。 若要更新元数据，请将数据库对象加载到中 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 若要迁移数据，必须重新连接到 DB2 和 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
**打开项目的步骤**  
  
1.  可使用下列过程之一：  
  
    -   在 " **文件** " 菜单上，指向 " **最近使用的项目**"，然后单击要打开的项目。  
  
    -   在 " **文件** " 菜单上，选择 " **打开项目**"，找到 "o2ssproj" 项目文件，选择该文件，然后单击 " **打开**"。  
  
2.  若要重新连接到 DB2，请在 " **文件** " 菜单上单击 " **重新连接到 db2**"。  
  
3.  若要重新连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，请在 " **文件** " 菜单上单击 " **重新连接" SQL Server**。  
  
## <a name="next-step"></a>下一步  
迁移过程的下一步是 [连接到 DB2 数据库](./connecting-to-db2-database-db2tosql.md)。  
  
## <a name="see-also"></a>另请参阅  
[将 DB2 数据库迁移到 SQL Server &#40;DB2ToSQL&#41;](../../ssma/db2/migrating-db2-databases-to-sql-server-db2tosql.md)  
[连接到 DB2 数据库 &#40;DB2ToSQL&#41;](../../ssma/db2/connecting-to-db2-database-db2tosql.md)  
[连接到 SQL Server &#40;DB2ToSQL&#41;](../../ssma/db2/connecting-to-sql-server-db2tosql.md)  
