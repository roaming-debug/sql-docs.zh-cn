---
title: 删除数据库中的数据文件或日志文件 | Microsoft Docs
description: 了解如何使用 SQL Server Management Studio 或 Transact-SQL 在 SQL Server 中删除数据或日志文件。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
helpviewer_keywords:
- logs [SQL Server], files
- deleting files
- removing files
- removing data
- data deletions [SQL Server]
- file deletion [SQL Server]
- deleting data
ms.assetid: 0db4018c-ce2c-4ba1-bb29-1e4f3791c925
author: stevestein
ms.author: sstein
ms.openlocfilehash: 062ac3ee839f62a2e789be180484d3d3e8a18d44
ms.sourcegitcommit: 00be343d0f53fe095a01ea2b9c1ace93cdcae724
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/26/2021
ms.locfileid: "98812945"
---
# <a name="delete-data-or-log-files-from-a-database"></a>删除数据库中的数据文件或日志文件
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  本主题说明如何使用 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 或 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 在 [!INCLUDE[tsql](../../includes/tsql-md.md)]中删除数据文件或日志文件。  
  
 **本主题内容**  
  
-   **开始之前：**  
  
     [先决条件](#Prerequisites)  
  
     [安全性](#Security)  
  
-   **删除数据库中的数据文件或日志文件，使用：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="prerequisites"></a><a name="Prerequisites"></a>先决条件  
  
-   文件必须为空后才能删除。 有关详细信息，请参阅 [收缩文件](../../relational-databases/databases/shrink-a-file.md)。  
  
###  <a name="security"></a><a name="Security"></a> Security  
  
####  <a name="permissions"></a><a name="Permissions"></a> 权限  
 需要对数据库拥有 ALTER 权限。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
  
#### <a name="to-delete-data-or-log-files-from-a-database"></a>删除数据库中的数据文件或日志文件  
  
1.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 的实例，然后展开该实例。  
  
2.  展开 **“数据库”** ，右键单击要从其中删除文件的数据库，再单击 **“属性”** 。  
  
3.  选择 **“文件”** 页。  
  
4.  在 **“数据库文件”** 网格中，选择要删除的文件，然后单击 **“删除”** 。  
  
5.  单击“确定”。  

##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
  
#### <a name="to-delete-data-or-log-files-from-a-database"></a>删除数据库中的数据文件或日志文件  
  
1.  连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行” 。 此示例删除文件 `test1dat4`。  
  
 [!code-sql[DatabaseDDL#AlterDatabase4](../../relational-databases/databases/codesnippet/tsql/delete-data-or-log-files_1.sql)]  
  
 有关更多示例，请参阅 [ALTER DATABASE 文件和文件组选项 (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-file-and-filegroup-options.md)。  
  
## <a name="see-also"></a>另请参阅  
 [收缩数据库](../../relational-databases/databases/shrink-a-database.md)   
 [向数据库中添加数据文件或日志文件](../../relational-databases/databases/add-data-or-log-files-to-a-database.md)  
  
  
