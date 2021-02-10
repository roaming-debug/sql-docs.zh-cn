---
title: 将 DB2 数据迁移到 SQL Server (DB2ToSQL) |Microsoft Docs
description: 在同步已转换的对象之后，了解如何将数据从 DB2 数据库迁移到 SQL Server 或 Azure SQL 数据库。
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 86cbd39f-6dac-409a-9ce1-7dd54403f84b
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: cd55ac5432784cb3af727fb923397fa9717dab40
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100017787"
---
# <a name="migrating-db2-data-into-sql-server-db2tosql"></a>将 DB2 数据迁移到 SQL Server (DB2ToSQL) 
使用成功同步转换后的对象后 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，可以将数据从 DB2 迁移到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
> [!IMPORTANT]  
> 如果正在使用的引擎是服务器端数据迁移引擎，则必须在运行 SSMA 的计算机上安装 DB2 扩展包和 DB2 提供程序的 SSMA，然后才能迁移数据。 SQL Server 代理服务也必须正在运行。 有关如何安装扩展包的详细信息，请参阅 [在 SQL Server 上安装 SSMA 组件](./installing-ssma-components-on-sql-server-db2tosql.md)  
  
## <a name="setting-migration-options"></a>设置迁移选项  
在将数据迁移到之前 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，请在 " **项目设置** " 对话框中查看项目迁移选项。  
  
-   使用此对话框，您可以设置迁移批大小、表锁定、约束检查、null 值处理和标识值处理等选项。 有关项目迁移设置的详细信息，请参阅 [项目设置 (迁移) ](./project-settings-migration-db2tosql.md)。  
  
-   使用 "**项目设置**" 对话框中的 **迁移引擎**，用户可以使用两种类型的数据迁移引擎来执行迁移过程：  
  
    1.  客户端数据迁移引擎  
  
    2.  服务器端数据迁移引擎  
  
**客户端数据迁移：**  
  
-   若要在客户端启动数据迁移，请在 "**项目设置**" 对话框中选择 "**客户端数据迁移引擎**" 选项。  
  
-   在 **项目设置** 中，设置了 **客户端数据迁移引擎** 选项。  
  
    > [!NOTE]  
    > **客户端数据迁移引擎** 驻留在 SSMA 应用程序中，因此不依赖于扩展包的可用性。  
  
**服务器端数据迁移：**  
  
-   在服务器端数据迁移期间，引擎驻留在目标数据库上。 它通过扩展包进行安装。 有关如何安装扩展包的详细信息，请参阅 [在 SQL Server 上安装 SSMA 组件](./installing-ssma-components-on-sql-server-db2tosql.md)  
  
-   若要在服务器端启动迁移，请在 "**项目设置**" 对话框中选择 "**服务器端数据迁移引擎**" 选项。  
  
## <a name="migrating-data-to-sql-server"></a>将数据迁移到 SQL Server  
迁移数据是一种大容量加载操作，用于将数据行从 DB2 表移入 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 事务中的表。 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 项目设置中配置每个事务中加载的行数。  
  
若要查看迁移消息，请确保输出窗格可见。 否则，请在 " **视图** " 菜单中选择 " **输出**"。  
  
**迁移数据**  
  
1.  检查下列各项：  
  
    -   DB2 提供程序安装在运行 SSMA 的计算机上。  
  
    -   已将转换的对象与数据库同步 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
2.  在 DB2 元数据资源管理器中，选择包含要迁移的数据的对象：  
  
    -   若要迁移所有架构的数据，请选中 " **架构**" 旁边的复选框。  
  
    -   若要迁移数据或省略单个表，请先展开架构，展开 " **表**"，然后选中或清除表旁边的复选框。  
  
3.  若要迁移数据，则会发生两种情况：  
  
    **客户端数据迁移：**  
  
    -   若要执行 **客户端数据迁移**，请在 "**项目设置**" 对话框中选择 "**客户端数据迁移引擎**" 选项。  
  
    **服务器端数据迁移：**  
  
    -   在服务器端执行数据迁移之前，请确保：  
  
        1.  在的实例上安装了 DB2 扩展包的 SSMA [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
        2.  SQL Server 代理服务正在 SQL Server 的实例上运行。  
  
    -   对于执行 **服务器端数据迁移**，请在 "**项目设置**" 对话框中选择 "**服务器端数据迁移引擎**" 选项。  
  
4.  在 DB2 元数据资源管理器中右键单击 " **架构** "，然后单击 " **迁移数据**"。 您还可以迁移各个对象或对象类别的数据：右键单击对象或其父文件夹;选择 " **迁移数据** " 选项。  
  
    > [!NOTE]  
    > 如果的实例上未安装 SSMA for DB2 扩展包 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，并且选择了 **服务器端数据迁移引擎** ，则在将数据迁移到目标数据库时，将遇到以下错误： "SSMA 数据迁移组件在 SQL Server 上找不到。 请检查是否正确安装了扩展包。 单击 " **取消** " 以终止数据迁移。  
  
5.  在 " **连接到 DB2** " 对话框中，输入连接凭据，然后单击 " **连接**"。 有关连接到 DB2 的详细信息，请参阅 [连接到 Db2 数据库 &#40;DB2ToSQL&#41;](../../ssma/db2/connecting-to-db2-database-db2tosql.md)  
  
    若要连接到目标数据库 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，请在 " **连接到 SQL Server** " 对话框中输入连接凭据，然后单击 " **连接**"。 有关连接到的详细信息 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，请参阅 [连接到 SQL Server](./connecting-to-sql-server-db2tosql.md)  
  
    消息将显示在 " **输出** " 窗格中。 迁移完成后，将显示 " **数据迁移" 报表** 。 如果任何数据未迁移，请单击包含错误的行，然后单击 " **详细信息**"。 完成报表后，单击 " **关闭**"。 有关数据迁移报表的详细信息，请参阅 [数据迁移报表 (SSMA Common) ](../sybase/data-migration-report-sybasetosql.md)  
  
> [!NOTE]  
> 当 SQL Express edition 用作目标数据库时，只允许客户端数据迁移，不支持服务器端数据迁移。  
  
## <a name="see-also"></a>另请参阅  
[将 DB2 数据迁移到 SQL Server &#40;DB2ToSQL&#41;](../../ssma/db2/migrating-db2-data-into-sql-server-db2tosql.md)  
