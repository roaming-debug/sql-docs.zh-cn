---
title: 更改数据库的配置设置 | Microsoft Docs
description: 了解如何使用 SQL Server Management Studio 或 Transact-SQL 在 SQL Server 2019 中更改数据库级别选项。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- database configuration [SQL Server]
- configuration options [SQL Server], databases
- modifying database configuration settings
ms.assetid: c29c3385-5043-400f-bb4e-044a4c9a9a4b
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: d3e1e312a539ccd3526e252facd523980d751e4b
ms.sourcegitcommit: 00be343d0f53fe095a01ea2b9c1ace93cdcae724
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/26/2021
ms.locfileid: "98813608"
---
# <a name="change-the-configuration-settings-for-a-database"></a>更改数据库的配置设置
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  本主题说明如何使用 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 或 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 在 [!INCLUDE[tsql](../../includes/tsql-md.md)]中更改服务器级别选项。 这些选项仅针对各自的数据库，而不会影响其他数据库。  
  
 **本主题内容**  
  
-   **开始之前：**  
  
     [限制和局限](#Restrictions)  
  
     [安全性](#Security)  
  
-   **若要更改数据库的选项设置，请使用：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> 限制和局限  
  
-   只有系统管理员、数据库所有者、 **sysadmin** 和 **dbcreator** 固定服务器角色成员以及 **db_owner** 固定数据库角色成员才能修改这些选项。  
  
###  <a name="security"></a><a name="Security"></a> Security  
  
####  <a name="permissions"></a><a name="Permissions"></a> 权限  
 需要对数据库拥有 ALTER 权限。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
  
#### <a name="to-change-the-option-settings-for-a-database"></a>更改数据库的选项设置  
  
1.  在对象资源管理器中，连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 实例，扩展该服务器，然后扩展“数据库”，右键单击某个数据库，再单击“属性”。  
  
2.  在 **“数据库属性”** 对话框中，单击 **“选项”** 访问大多数配置设置。 文件和文件组配置、镜像和日志传送都在各自相应的页上。  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
  
#### <a name="to-change-the-option-settings-for-a-database"></a>更改数据库的选项设置  
  
1.  连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行” 。 该示例设置 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 示例数据库的恢复模式和数据页面验证选项。  
  
 [!code-sql[DatabaseDDL#AlterDatabase7](../../relational-databases/databases/codesnippet/tsql/change-the-configuration_1.sql)]  
  
 详细信息，请参阅 [ALTER DATABASE SET 选项 (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-set-options.md)。  
  
## <a name="see-also"></a>另请参阅  
 [ALTER DATABASE 兼容级别 (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md)   
 [ALTER DATABASE 数据库镜像 (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-database-mirroring.md)   
 [ALTER DATABASE SET HADR (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-set-hadr.md)   
 [重命名数据库](../../relational-databases/databases/rename-a-database.md)   
 [收缩数据库](../../relational-databases/databases/shrink-a-database.md)  
  
  
