---
description: '将 Sybase ASE 数据迁移到 SQL Server-Azure SQL 数据库 (SybaseToSQL) '
title: 将 Sybase ASE 数据迁移到 SQL Server-Azure SQL 数据库 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Migrating data,Client Side Data Migration
- Migrating data,Server Side Data Migration
ms.assetid: 54a39f5e-9250-4387-a3ae-eae47c799811
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 0406997ade10e6ddfc9b13c76240973662255f78
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100076028"
---
# <a name="migrating-sybase-ase-data-into-sql-server---azure-sql-database--sybasetosql"></a>将 Sybase ASE 数据迁移到 SQL Server-Azure SQL 数据库 (SybaseToSQL) 
成功将 Sybase 自适应服务器企业 (ASE) 数据库对象加载到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 AZURE Sql 数据库后，可以将数据从 ASE 迁移到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 Azure sql 数据库。  
  
> [!IMPORTANT]  
> 如果正在使用的引擎是服务器端数据迁移引擎，则在迁移数据之前，必须在运行 SSMA 的计算机上安装适用于 Sybase ASE 扩展包和 Sybase ASE 提供程序的 SSMA。 SQL Server 代理服务也必须正在运行。 有关如何安装扩展包的详细信息，请参阅 [在 SQL Server 上安装 SSMA 组件 (SybaseToSQL) ](./installing-ssma-components-on-sql-server-sybasetosql.md)  
  
## <a name="setting-migration-options"></a>设置迁移选项  
在将数据迁移到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 AZURE SQL 数据库之前，请在 " **项目设置** " 对话框中查看项目迁移选项。  
  
-   使用此对话框，您可以设置迁移批大小、表锁定、约束检查、null 值处理和标识值处理等选项。 有关项目迁移设置的详细信息，请参阅 [ (Sybase) 的项目设置 (迁移) ](./project-settings-migration-sybasetosql.md)。  
  
    有关 **扩展数据迁移设置** 的详细信息，请参阅 [数据迁移设置](data-migration-settings-sybasetosql.md)  
  
-   使用 "**项目设置**" 对话框中的 **迁移引擎**，用户可以使用两种类型的数据迁移引擎（即）执行迁移过程：  
  
    1.  客户端数据迁移引擎  
  
    2.  服务器端数据迁移引擎  
  
**客户端数据迁移：**  
  
-   若要在客户端启动数据迁移，请在 "**项目设置**" 对话框中选择 "**客户端数据迁移引擎**" 选项。  
  
-   在 " **项目设置**" 中，默认情况下会设置 **客户端数据迁移引擎** 选项。  
  
    > [!NOTE]  
    > Client-Side 数据迁移引擎驻留在 SSMA 应用程序中，因此不依赖于扩展包的可用性。  
  
**服务器端数据迁移：**  
  
-   在服务器端数据迁移期间，引擎驻留在目标数据库上。 它通过扩展包进行安装。 有关如何安装扩展包的详细信息，请参阅 [在 SQL Server 上安装 SSMA 组件 (SybaseToSQL) ](./installing-ssma-components-on-sql-server-sybasetosql.md)  
  
-   若要在服务器端启动迁移，请在 "**项目设置**" 对话框中选择 "**服务器端数据迁移引擎**" 选项。  
  
> [!NOTE]  
> 将 Azure SQL 数据库用作目标数据库时，只允许 **客户端数据迁移** ，不支持服务器端数据迁移。  
  
## <a name="migrating-data-to-sql-server-or-azure-sql-database"></a>将数据迁移到 SQL Server 或 Azure SQL 数据库  
迁移数据是一种大容量加载操作，它将行中的数据行移入事务中的 SQL Server 表。 在项目设置中配置每个事务中加载到 SQL Server 或 Azure SQL 数据库中的行数。  
  
若要查看迁移消息，请确保输出窗格可见。 否则，请在 "**视图**" 菜单中选择 "**输出**"。  
  
**迁移数据**  
  
1.  检查下列各项：  
  
    -   ASE 提供程序安装在运行 SSMA 的计算机上。  
  
    -   已将转换的对象与目标数据库 (SQL Server 或 Azure SQL 数据库) 同步。  
  
2.  在 "Sybase 元数据资源管理器" 中，选择包含要迁移的数据的对象：  
  
    -   若要迁移所有架构的数据，请选中 " **架构**" 旁边的复选框。  
  
    -   若要迁移数据或省略单个表，请先展开架构，展开 " **表**"，然后选中或清除表旁边的复选框。  
  
3.  若要迁移数据，则会发生两种情况：  
  
    **客户端数据迁移：**  
  
    若要执行 **客户端数据迁移**，请在 "**项目设置**" 对话框中选择 "**客户端数据迁移引擎**" 选项。  
  
    **服务器端数据迁移：**  
  
    -   在执行服务器端数据迁移之前，请确保：  
  
        1.  SQL Server 的实例上安装了 Sybase 扩展包的 SSMA。  
  
        2.  SQL Server 代理服务在实例上运行 SQL Server  
  
    -   对于执行 **服务器端数据迁移**，请在 "**项目设置**" 对话框中选择 "**服务器端数据迁移引擎**" 选项。  
  
4.  右键单击 "Sybase 元数据资源管理器" 中的 " **架构** "，然后单击 " **迁移数据**"。 您还可以迁移各个对象或对象类别的数据：右键单击对象或其父文件夹，然后选择 " **迁移数据** " 选项。  
  
    > [!NOTE]  
    > 如果 SQL Server 的实例上未安装 SSMA for Sybase 扩展包，并且选择了 **服务器端数据迁移引擎** ，则在将数据迁移到目标数据库时，将遇到以下错误： "SSMA 数据迁移组件在 SQL Server 上找不到。 请检查是否正确安装了扩展包。 单击 " **取消** " 以终止数据迁移。  
  
5.  在 " **连接到 SYBASE ASE** " 对话框中，输入连接凭据，然后单击 " **连接**"。 有关连接到 Sybase ASE 的详细信息，请参阅 [连接到 sybase &#40;SybaseToSQL&#41;](../../ssma/sybase/connect-to-sybase-sybasetosql.md)  
  
    如果目标数据库 SQL Server，请在 " **连接到 SQL Server** " 对话框中输入连接凭据，然后单击 " **连接**"。 有关连接到 SQL Server 的详细信息，请参阅 [连接到 SQL Server (SybaseToSQL) ](./connecting-to-sql-server-sybasetosql.md)  
  
    如果目标数据库是 Azure SQL 数据库，则在 " **连接到 AZURE Sql 数据库** " 对话框中输入连接凭据，并单击 " **连接**"。 有关连接到 Azure SQL 数据库的详细信息，请参阅 [连接到 AZURE Sql 数据库 &#40;SybaseToSQL&#41;](../../ssma/sybase/connecting-to-azure-sql-db-sybasetosql.md)  
  
    消息将显示在 " **输出** " 窗格中。 迁移完成后，将显示 " **数据迁移" 报表** 。 如果任何数据未迁移，请单击包含错误的行，然后单击 " **详细信息**"。 完成报表后，单击 " **关闭**"。 有关数据迁移报表的详细信息，请参阅 [数据迁移报表 (SSMA Common) ](./data-migration-report-sybasetosql.md)  
  
> [!NOTE]  
> 当 SQL Express edition 用作目标数据库时，只允许客户端数据迁移，不支持服务器端数据迁移。  
  
## <a name="see-also"></a>另请参阅  
[将 Sybase ASE 数据库迁移到 SQL Server-Azure SQL 数据库 &#40;SybaseToSQL&#41;](../../ssma/sybase/migrating-sybase-ase-databases-to-sql-server-azure-sql-db-sybasetosql.md)  
