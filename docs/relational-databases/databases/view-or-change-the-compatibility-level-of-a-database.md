---
title: 查看或更改数据库的兼容级别 | Microsoft Docs
description: 了解如何使用 SQL Server Management Studio 或 Transact-SQL 在 SQL Server 中查看或更改数据库的兼容级别。
ms.custom: ''
ms.date: 11/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
helpviewer_keywords:
- compatibility levels [SQL Server], viewing
- compatibility [SQL Server], databases
- compatibility levels [SQL Server], changing
ms.assetid: 579867ec-57cb-4cb8-af35-9688c1e9e15d
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 8b513694dacf477f0fde5d90ee9be6952158aa63
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97481418"
---
# <a name="view-or-change-the-compatibility-level-of-a-database"></a>查看或更改数据库的兼容级别
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  本主题说明如何使用 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 或 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 在 [!INCLUDE[tsql](../../includes/tsql-md.md)]中查看或更改数据库的兼容级别。 在更改数据库的兼容级别之前，应先了解此更改对应用程序的影响。 有关详细信息，请参阅 [ALTER DATABASE 兼容级别 (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md)。  
  
 **本主题内容**  
  
-   **开始之前：**  
  
     [安全性](#Security)  
  
-   **查看或更改数据库的兼容级别，使用：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="security"></a><a name="Security"></a> Security  
  
####  <a name="permissions"></a><a name="Permissions"></a> 权限  
 需要对数据库拥有 ALTER 权限。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
  
#### <a name="to-view-or-change-the-compatibility-level-of-a-database"></a>查看或更改数据库的兼容级别  
  
1.  连接到 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]的相应实例之后，在对象资源管理器中，单击服务器名称。  
  
2.  展开 **“数据库”** ，然后根据数据库的不同，选择用户数据库，或展开 **“系统数据库”** ，再选择系统数据库。  
  
3.  右键单击数据库，再单击“属性”。  
  
     **“数据库属性”** 对话框将打开。  
  
4.  在 **“选择页”** 窗格中，单击 **“选项”** 。  
  
     当前兼容级别显示在 **“兼容级别”** 列表框中。  
  
5.  若要更改兼容级别，请从列表中选择其他选项。 [ALTER DATABASE 兼容性级别 (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md#supported-dbcompats) 页中列出了不同 [!INCLUDE[ssde_md](../../includes/ssde_md.md)] 版本的可用选项。  

##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
  
#### <a name="to-view-the-compatibility-level-of-a-database"></a>查看数据库的兼容级别  
  
1.  连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行” 。 此示例将返回 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库的兼容级别。  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT compatibility_level  
FROM sys.databases WHERE name = 'AdventureWorks2012';  
GO  
```  
  
#### <a name="to-change-the-compatibility-level-of-a-database"></a>更改数据库的兼容级别  
  
1.  连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行” 。 此示例将 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库的兼容级别更改为 `120`，这是 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 的兼容级别。  
  
```sql  
ALTER DATABASE AdventureWorks2012  
SET COMPATIBILITY_LEVEL = 120;  
GO  
```  
  
## <a name="see-also"></a>另请参阅
 [ALTER DATABASE (Transact-SQL) 兼容性级别](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md)
