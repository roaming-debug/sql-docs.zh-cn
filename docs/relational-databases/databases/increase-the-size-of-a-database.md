---
description: 增加数据库的大小
title: 增加数据库的大小 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- databases [SQL Server], size
- increasing database size
- database size [SQL Server], increasing
- size [SQL Server], databases
ms.assetid: 14f4206d-3afa-4ba9-9849-23e81d63306d
author: stevestein
ms.author: sstein
monikerRange: '>=sql-server-2016||>=sql-server-linux-2017'
ms.openlocfilehash: 595aa1cba75782ebdfa8e17f88db6887014c7767
ms.sourcegitcommit: 00be343d0f53fe095a01ea2b9c1ace93cdcae724
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/26/2021
ms.locfileid: "98812946"
---
# <a name="increase-the-size-of-a-database"></a>增加数据库的大小
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  本主题说明如何使用 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 或 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 在 [!INCLUDE[tsql](../../includes/tsql-md.md)]中增加数据库的大小。 通过增加现有数据或日志文件的大小或向数据库添加新文件，可以扩展数据库。  
  
 **本主题内容**  
  
-   **开始之前：**  
  
     [限制和局限](#Restrictions)  
  
     [安全性](#Security)  
  
-   **增加数据库的大小，使用：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> 限制和局限  
  
-   当 BACKUP 语句正在运行时，不能添加或删除文件。  
  
###  <a name="security"></a><a name="Security"></a> Security  
  
####  <a name="permissions"></a><a name="Permissions"></a> 权限  
 需要对数据库拥有 ALTER 权限。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
  
#### <a name="to-increase-the-size-of-a-database"></a>增加数据库的大小  
  
1.  在 **对象资源管理器** 中，连接到 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]的实例，然后展开该实例。  
  
2.  展开“数据库”，右键单击要扩展的数据库，再单击“属性”。  
  
3.  在 **“数据库属性”** 中，选择 **“文件”** 页。  
  
4.  若要增加现有文件的大小，请增加文件的“初始大小 (MB)”列中的值。 数据库的大小必须至少增加 1 MB。  
  
5.  若要通过添加新文件增加数据库大小，请单击 **“添加”** ，然后输入新文件的值。 有关详细信息，请参阅 [向数据库中添加数据文件或日志文件](../../relational-databases/databases/add-data-or-log-files-to-a-database.md)。  
  
6.  单击“确定”。  

##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
  
#### <a name="to-increase-the-size-of-a-database"></a>增加数据库的大小  
  
1.  连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行” 。 此示例可增加文件 `test1dat3`的大小。  
  
 [!code-sql[DatabaseDDL#AlterDatabase5](../../relational-databases/databases/codesnippet/tsql/increase-the-size-of-a-d_1.sql)]  
  
 有关更多示例，请参阅 [ALTER DATABASE 文件和文件组选项 (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-file-and-filegroup-options.md)。  
  
## <a name="see-also"></a>另请参阅  
 [向数据库中添加数据文件或日志文件](../../relational-databases/databases/add-data-or-log-files-to-a-database.md)   
 [收缩数据库](../../relational-databases/databases/shrink-a-database.md)  
  
  
