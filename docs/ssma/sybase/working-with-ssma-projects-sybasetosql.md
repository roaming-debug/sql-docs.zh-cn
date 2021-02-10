---
description: 处理 SSMA 项目 (SybaseToSQL)
title: 使用 SSMA 项目 (SybaseToSQL) |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 11091d95-c488-48c3-891a-743cac94ac93
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: b0276efc988eca1674410c31a2f83379ee1e875b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100064972"
---
# <a name="working-with-ssma-projects-sybasetosql"></a>处理 SSMA 项目 (SybaseToSQL)
若要将 Sybase 自适应服务器企业 (ASE) 数据库迁移到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 SQL Azure，请先创建一个 SSMA 项目。 项目是一个文件，其中包含有关要迁移到或 SQL Azure 的 ASE 数据库的元数据 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或将接收迁移对象和数据的 SQL Azure 的元数据， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或者 SQL Azure 连接信息和项目设置。  
  
打开某个项目时，它会断开与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 SQL Azure 的连接。 这使你可以脱机工作。 您可以重新连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 SQL Azure。 有关详细信息，请参阅连接[到 SQL Server &#40;SybaseToSQL&#41;](../../ssma/sybase/connecting-to-sql-server-sybasetosql.md)  /  [连接到 Azure SQL 数据库 &#40;SybaseToSQL&#41;](../../ssma/sybase/connecting-to-azure-sql-db-sybasetosql.md)。  
  
## <a name="reviewing-default-project-settings"></a>查看默认项目设置  
SSMA 包含多个选项，用于转换和加载数据库对象、迁移数据，以及通过 ASE 和 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 SQL Azure 同步 SSMA。 这些选项的默认设置适用于许多用户。 但是，在创建新的 SSMA 项目之前，应查看选项，如果需要，请更改将用于所有新项目的默认值。  
  
**查看默认项目设置**  
  
1.  在 " **工具** " 菜单上，选择 " **默认项目设置**"。  
  
2.  在 " **迁移目标版本** " 下拉顺序中选择需要查看或更改其设置的项目类型，然后单击 " **常规** " 选项卡。  
  
3.  在左窗格中，单击 " **转换**"。  
  
4.  在右侧窗格中，查看选项，根据需要更改选项。 有关这些选项的详细信息，请参阅 [SybaseToSQL&#41;&#40;转换&#41; &#40;项目设置 ](../../ssma/sybase/project-settings-conversion-sybasetosql.md)。  
  
5.  重复步骤1-3 迁移、SQL Azure、加载对象、GUI 和类型映射页。  
  
    -   有关迁移选项的信息，请参阅 [项目设置 &#40;迁移&#41; &#40;SybaseToSQL&#41;](../../ssma/sybase/project-settings-migration-sybasetosql.md)。  
  
    -   有关用于将对象加载到的选项的信息 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，请参阅 [项目设置 &#40;同步&#41; &#40;SybaseToSQL&#41;](../../ssma/sybase/project-settings-synchronization-sybasetosql.md)。  
  
    -   有关 GUI 选项的详细信息，请参阅 [项目设置 &#40;GUI&#41; &#40;SybaseToSQL&#41;](../../ssma/sybase/project-settings-gui-sybasetosql.md)。  
  
    -   有关数据类型映射设置的详细信息，请单击 " [项目设置" &#40;类型映射 "&#41; &#40;SybaseToSQL&#41;](../../ssma/sybase/project-settings-type-mapping-sybasetosql.md)"。  
  
    -   有关 SQL Azure 选项的详细信息，请参阅 [AZURE SQL Database &#40;的项目设置 &#41; &#40;SybaseToSQL&#41;](../../ssma/sybase/project-settings-azure-sql-db-sybasetosql.md)。  
  
    > [!NOTE]  
    > 只有当你在创建项目时选择 " **迁移到 SQL Azure** " 时，才会显示 SQL Azure 设置。  
  
## <a name="creating-new-projects"></a>创建新项目  
若要将数据从 ASE 数据库迁移到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 SQL Azure，必须先创建一个项目。  
  
**创建项目**  
  
1.  在“文件”菜单中，选择“新建项目”。  
  
    将显示“新建项目”对话框。  
  
2.  在 " **名称** " 框中，输入项目的名称。  
  
3.  在 " **位置** " 框中，为项目输入或选择一个文件夹。  
  
4.  在 " **迁移到** " 下拉菜单中，选择用于迁移的目标版本 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 可用选项包括：  
  
    -   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2005  
  
    -   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2008  
  
    -   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2012  
  
    -   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2014  
  
    -   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2016  
  
    -   Azure SQL 数据库  
  
然后单击 **"确定"**。  
  
## <a name="customizing-project-settings"></a>自定义项目设置  
除了定义适用于所有新 SSMA 项目的默认项目设置，你还可以自定义每个项目的设置。 有关详细信息，请参阅 [设置项目选项 &#40;SybaseToSQL&#41;](../../ssma/sybase/setting-project-options-sybasetosql.md)。  
  
在源数据库和目标数据库之间自定义数据类型映射时，可以在项目、数据库或对象级别定义映射。 有关类型映射的详细信息，请参阅将 [SYBASE ASE 和 SQL Server 数据类型映射 &#40;SybaseToSQL&#41;](../../ssma/sybase/mapping-sybase-ase-and-sql-server-data-types-sybasetosql.md)。  
  
## <a name="saving-projects"></a>保存项目  
保存项目时，SSMA 会将项目设置和数据库元数据保留到项目文件中。  
  
**保存项目的步骤**  
  
-   在 " **文件** " 菜单上，选择 " **保存项目**"。  
  
    如果项目中的数据库已更改或尚未转换，SSMA 会提示你将元数据保存到项目中。 保存元数据将允许脱机工作，并将完整的项目文件发送给其他人，包括技术支持人员。 如果系统提示你保存元数据，请执行以下操作：  
  
    1.  对于显示 " **缺少元数据**" 状态的每个数据库，请选中数据库名称旁边的复选框。  
  
        保存元数据可能需要几分钟时间。 如果此时不想保存元数据，请不要选中任何复选框。  
  
    2.  单击“保存”按钮  。  
  
        SSMA 将分析 Sybase ASE 架构，并将元数据保存到项目文件。  
  
## <a name="opening-projects"></a>打开项目  
打开某个项目时，该项目将从 ASE 断开连接，并与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 SQL Azure 断开连接。 这使你可以脱机工作。 若要更新元数据，请将数据库对象加载到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 SQL Azure。 若要迁移数据，必须重新连接到 ASE， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 SQL Azure。  
  
**打开项目的步骤**  
  
1.  可使用下列过程之一：  
  
    -   在 " **文件** " 菜单上，指向 " **最近使用的项目**"，然后选择要打开的项目。  
  
    -   在 " **文件** " 菜单上，选择 " **打开项目**"，找到 "s2ssproj" 项目文件，选择该文件，然后单击 " **打开**"。  
  
2.  若要重新连接到 ASE，请在 " **文件** " 菜单上，选择 " **重新连接到 Sybase**"。  
  
3.  若要重新连接 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 SQL Azure，请在 "**文件**" 菜单上选择 "**重新连接" SQL Server**  /  **重新连接到 SQL Azure**。  
  
## <a name="next-step"></a>下一步  
迁移过程的下一步是 [连接到 SYBASE ASE](connecting-to-sybase-ase-sybasetosql.md)。  
  
## <a name="see-also"></a>另请参阅  
[将 Sybase ASE 数据库迁移到 SQL Server-Azure SQL 数据库 &#40;SybaseToSQL&#41;](../../ssma/sybase/migrating-sybase-ase-databases-to-sql-server-azure-sql-db-sybasetosql.md)  
[连接到 Sybase ASE &#40;SybaseToSQL&#41;](../../ssma/sybase/connecting-to-sybase-ase-sybasetosql.md)  
[连接到 SQL Server &#40;SybaseToSQL&#41;](../../ssma/sybase/connecting-to-sql-server-sybasetosql.md)  
[连接到 Azure SQL 数据库 &#40;SybaseToSQL&#41;](../../ssma/sybase/connecting-to-azure-sql-db-sybasetosql.md)  
  
