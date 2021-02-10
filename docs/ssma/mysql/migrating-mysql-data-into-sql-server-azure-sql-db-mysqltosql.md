---
description: '将 MySQL 数据迁移到 SQL Server-Azure SQL 数据库 (MySQLToSQL) '
title: 将 MySQL 数据迁移到 SQL Server-Azure SQL 数据库 (MySQLToSQL) |Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Data Migration, server side data migration
- Data Migration,client side data migration
ms.assetid: a6a7f4d6-68aa-4a38-93bf-53eba0d7dc82
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 3207dc38dd777ee6ccc36e37e9b18414dbea085e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100017521"
---
# <a name="migrating-mysql-data-into-sql-server---azure-sql-database-mysqltosql"></a>将 MySQL 数据迁移到 SQL Server-Azure SQL 数据库 (MySQLToSQL) 
使用或 SQL Azure 成功同步转换后的对象后 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，可以将数据从 MySQL 迁移到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 SQL Azure。  
  
> [!IMPORTANT]  
> 如果正在使用的引擎是服务器端数据迁移引擎，则在迁移数据之前，必须在运行 SSMA 的计算机上安装 SSMA for MySQL Extension Pack 和 MySQL 提供程序。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]代理服务还必须正在运行。 有关如何安装扩展包的详细信息，请参阅 [在 SQL Server 上安装 SSMA 组件 (MySQL 到 SQL) ](./installing-ssma-components-on-sql-server-mysqltosql.md)  
  
## <a name="setting-migration-options"></a>设置迁移选项  
在将数据迁移到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 SQL Azure 之前，请在 " **项目设置** " 对话框中查看项目迁移选项。  
  
-   使用此对话框可以设置迁移批大小、表锁定、约束检查、null 值处理和标识值处理等选项。 有关项目迁移设置的详细信息，请参阅 [项目设置 (迁移) ](./project-settings-migration-mysqltosql.md)。  
  
    有关 **扩展数据迁移设置** 的详细信息，请参阅 [数据迁移设置](data-migration-settings-mysqltosql.md)  
  
-   使用 "**项目设置**" 对话框中的 **迁移引擎**，用户可以使用两种类型的数据迁移引擎来执行迁移过程：  
  
    1.  客户端数据迁移引擎  
  
    2.  服务器端数据迁移引擎  
  
**客户端数据迁移：**  
  
-   若要在客户端启动数据迁移，请在 "**项目设置**" 对话框中选择 "**客户端数据迁移引擎**" 选项。  
  
-   在 **项目设置** 中，设置了 **客户端数据迁移引擎** 选项。  
  
    > [!NOTE]  
    > **客户端数据迁移引擎** 驻留在 SSMA 应用程序中，因此不依赖于扩展包的可用性。  
  
**服务器端数据迁移：**  
  
-   在服务器端数据迁移期间，引擎驻留在目标数据库上。 它通过扩展包进行安装。 有关如何安装扩展包的详细信息，请参阅 [在 SQL Server 上安装 SSMA 组件 (MySQL 到 SQL) ](./installing-ssma-components-on-sql-server-mysqltosql.md)  
  
-   若要在服务器端启动迁移，请在 "**项目设置**" 对话框中选择 "**服务器端数据迁移引擎**" 选项。  
  
> [!IMPORTANT]  
> **客户端数据迁移** 选项仅适用于 SQL Azure。  
  
## <a name="migrating-data-to-sql-server-or-sql-azure"></a>将数据迁移到 SQL Server 或 SQL Azure  
迁移数据是一种大容量加载操作，用于将数据行从 MySQL 表移入 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 SQL Azure 事务中的表。 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 项目设置中配置每个事务中加载的行数。  
  
若要查看迁移消息，请确保输出窗格可见。 否则，请在 " **视图** " 菜单中选择 " **输出**"。  
  
**迁移数据**  
  
1.  检查下列各项：  
  
    -   MySQL 提供程序安装在运行 SSMA 的计算机上。  
  
    -   已将转换的对象与目标数据库同步 (SQL Server/SQL Azure) 。  
  
2.  在 MySQL 元数据资源管理器中，选择包含要迁移的数据的对象：  
  
    -   若要迁移所有架构的数据，请选中 " **架构**" 旁边的复选框。  
  
    -   若要迁移数据或省略单个表，请先展开架构，展开 " **表**"，然后选中或清除表旁边的复选框。  
  
3.  若要迁移数据，则会发生两种情况：  
  
    **客户端数据迁移：**  
  
    -   若要执行 **客户端数据迁移**，请在 "**项目设置**" 对话框中选择 "**客户端数据迁移引擎**" 选项。  
  
    **服务器端数据迁移：**  
  
    -   在服务器端执行数据迁移之前，请确保：  
  
        1.  SQL Server 的实例上安装了 MySQL 扩展包的 SSMA。  
  
        2.  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]代理服务在的实例上运行 SQL Server  
  
    -   对于执行 **服务器端数据迁移**，请在 "**项目设置**" 对话框中选择 "**服务器端数据迁移引擎**" 选项。  
  
4.  在 MySQL 元数据资源管理器中右键单击 " **架构** "，然后单击 " **迁移数据**"。 您还可以迁移各个对象或对象类别的数据：右键单击对象或其父文件夹;选择 " **迁移数据** " 选项。  
  
    > [!NOTE]  
    > 如果 SQL Server 的实例上未安装 SSMA for MySQL 扩展包，并且选择了 **服务器端数据迁移引擎** ，则在将数据迁移到目标数据库时，将遇到以下错误： "SSMA SQL Server 上找不到数据迁移组件"，将无法进行服务器端数据迁移。 请检查是否正确安装了扩展包。 单击 " **取消** " 以终止数据迁移。  
  
5.  在 " **连接到 MySQL** " 对话框中，输入连接凭据，然后单击 " **连接**"。 有关连接到 MySQL 的详细信息，请参阅 [连接到 mysql &#40;MySQLToSQL&#41;](../../ssma/mysql/connect-to-mysql-mysqltosql.md)  
  
    如果目标数据库 SQL Server，请在 " **连接到 SQL Server** " 对话框中输入连接凭据，然后单击 " **连接**"。 有关连接到 SQL Server 的详细信息，请参阅 [连接到 SQL Server](../sybase/connecting-to-sql-server-sybasetosql.md)  
  
    如果目标数据库 SQL Azure，请在 " **连接到 SQL Azure** " 对话框中输入连接凭据，并单击 " **连接**"。 有关连接到 SQL Azure 的详细信息，请参阅 [连接到 AZURE SQL 数据库 &#40;MySQLToSQL&#41;](../../ssma/mysql/connect-to-azure-sql-db-mysqltosql.md)  
  
    消息将显示在 " **输出** " 窗格中。 迁移完成后，将显示 " **数据迁移" 报表** 。 如果任何数据未迁移，请单击包含错误的行，然后单击 " **详细信息**"。 完成报表后，单击 " **关闭**"。 有关数据迁移报表的详细信息，请参阅 [数据迁移报表 (SSMA Common) ](../sybase/data-migration-report-sybasetosql.md)  
  
> [!NOTE]  
> 当 SQL Express edition 用作目标数据库时，只允许客户端数据迁移，不支持服务器端数据迁移。  
  
## <a name="see-also"></a>另请参阅  
[将 MySQL 数据库迁移到 SQL Server-Azure SQL 数据库 &#40;MySQLToSql&#41;](../../ssma/mysql/migrating-mysql-databases-to-sql-server-azure-sql-db-mysqltosql.md)  
