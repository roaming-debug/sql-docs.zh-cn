---
title: 将 MySQL 数据库映射到 SQL Server 架构 (MySQLToSQL) |Microsoft Docs
description: 了解如何为 MySQL 架构与 SQL Server 或 Azure SQL 数据库之间的 MySQL 映射自定义 SSMA 或接受默认值。
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Mapping, Modifying target database and schema
- Mapping, reverting to default database and schema
ms.assetid: 5c6fb445-92ae-4933-b77d-80230931c024
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 44ac691a0fe9cb4b513a2f9b435457c699c7e3e9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100076068"
---
# <a name="mapping-mysql-databases-to-sql-server-schemas-mysqltosql"></a>将 MySQL 数据库映射到 SQL Server 架构 (MySQLToSQL)
默认情况下，SSMA for MySQL 将 MySQL 架构中的所有对象迁移到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 名为的或 AZURE SQL 数据库的架构。 但是，你可以自定义 MySQL 架构和 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 AZURE SQL 数据库之间的映射。  
  
## <a name="mysql-and-sql-server-or-sql-azure-schemas"></a>MySQL、SQL Server 或 SQL Azure 架构  
架构的 MySQL 概念映射到数据库的 SQL Server 概念及其架构之一。 SSMA 是指作为架构的数据库和架构的 SQL Server 组合。  
  
架构的 MySQL 概念映射到数据库的 SQL Server 概念及其架构之一。 例如，MySQL 可能有一个名为 **HR** 的架构。 SQL Server 的实例可能有一个名为 **HR** 的数据库，该数据库中的数据库是架构。 一个架构是 **dbo** (或数据库所有者) 架构。 默认情况下，MySQL schema **hr** 将映射到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库，架构 **hr. dbo**。 SSMA 是指 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 作为架构的数据库和架构的组合。  
  
可以修改 MySQL 和 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 Azure 架构之间的映射。  
  
## <a name="modifying-the-target-database-and-schema"></a>修改目标数据库和架构  
在 SSMA 中，可以将 MySQL 架构映射到任何可用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 SQL Azure 架构。  
  
**修改数据库和架构**  
  
1.  在 MySQL 元数据资源管理器中，选择 " **架构**"。  
  
    选择各个架构时，还可以使用 " **架构映射** " 选项卡。 为所选对象自定义 " **架构映射** " 选项卡中的列表。  
  
2.  在右侧窗格中，单击 " **架构映射** " 选项卡。  
  
    你将看到所有 MySQL 架构的列表，后跟目标值。 此目标以两部分表示法表示， (*database. schema*) [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 SQL Azure 其中将迁移对象和数据。  
  
3.  选择包含要更改的映射的行，然后单击 " **修改**"。  
  
    在 " **选择目标架构** " 对话框中，您可以浏览可用目标数据库和架构，或在两部分表示形式的文本框中输入数据库和架构名称 (database. Schema) ，然后单击 **"确定"**。  
  
4.  " **架构映射** " 选项卡上的目标更改。  
  
**映射模式**  
  
-   映射到 SQL Server  
  
您可以将源数据库映射到任何目标数据库。 默认情况下，源数据库映射到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 使用 SSMA 连接的目标数据库。 如果要映射的目标数据库在上不 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 存在，则系统会提示你提供消息 **"目标元数据中不存在数据库和/或架构 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。它将在同步过程中创建。是否要继续？ "** 单击“是”。 同样，你可以将架构映射到目标数据库下将在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 同步过程中创建的非现有架构。  
  
-   映射到 SQL Azure  
  
您可以将源数据库映射到连接目标数据库 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或连接的目标数据库中的任何架构 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 如果将源架构映射到已连接目标数据库下的任何非现有架构，则系统会提示您输入消息 **"目标元数据中不存在架构"。它将在同步过程中创建。是否要继续？"** 单击" 是 "。  
  
## <a name="reverting-to-the-default-database-and-schema"></a>恢复到默认数据库和架构  
如果自定义 MySQL 架构与 SQL Server 架构之间的映射，则可以将映射恢复为默认值。  
  
**恢复为默认数据库和架构**  
  
1.  在 "架构映射" 选项卡下，选择任意行，然后单击 " **重置为默认值** " 以还原为默认数据库和架构。  
  
## <a name="next-steps"></a>后续步骤  
如果要分析 MySQL 对象到 SQL Server 或 SQL Azure 对象的转换，则可以 [创建转换报告](assessing-mysql-databases-for-conversion-mysqltosql.md) ，否则可 [将 mysql 数据库对象定义转换](converting-mysql-databases-mysqltosql.md) 为 SQL Server 或 SQL Azure 架构  
  
## <a name="see-also"></a>另请参阅  
[&#40;转换的项目设置&#41; &#40;MySQLToSQL&#41;](../../ssma/mysql/project-settings-conversion-mysqltosql.md)  
[连接到 Azure SQL 数据库 &#40;MySQLToSQL&#41;](../../ssma/mysql/connecting-to-azure-sql-db-mysqltosql.md)  
[将 MySQL 数据库迁移到 SQL Server-Azure SQL 数据库 &#40;MySQLToSql&#41;](../../ssma/mysql/migrating-mysql-databases-to-sql-server-azure-sql-db-mysqltosql.md)  
[连接到 SQL Server &#40;MySQLToSQL&#41;](../../ssma/mysql/connecting-to-sql-server-mysqltosql.md)  
  
