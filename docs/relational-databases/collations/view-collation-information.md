---
description: 查看排序规则信息
title: 查看排序规则信息 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- collations [SQL Server], view
ms.assetid: 1338b4ea-7142-44bc-a3b9-44e54431405f
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 02cd93e23233d62ee28327e90742593cf6f0bf07
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97460560"
---
# <a name="view-collation-information"></a>查看排序规则信息
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
    
<a name="Top"></a> 您可以通过使用“对象资源管理器”菜单选项或使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 查看 [!INCLUDE[tsql](../../includes/tsql-md.md)]中的服务器、数据库或列的排序规则。  
  
##  <a name="how-to-view-a-collation-setting"></a><a name="Procedures"></a> 如何查看排序规则设置  
 您可以使用以下项之一：  
  
-   [SQL Server Management Studio](#SSMSProcedure)  
  
-   [Transact-SQL](#TsqlProcedure)  
  
###  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
 **使用对象资源管理器查看服务器（SQL Server 实例）的排序规则设置**  
  
1.  在“对象资源管理器”中，连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]的实例。  
  
2.  右键单击该实例，然后选择“属性”。  
  
 **使用对象资源管理器查看数据库的排序规则设置**  
  
1.  在对象资源管理器中，连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 的实例，然后展开该实例。  
  
2.  展开“数据库”，右键单击数据库，然后选择“属性”。  
  
 **使用对象资源管理器查看列的排序规则设置**  
  
1.  在对象资源管理器中，连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 的实例，然后展开该实例。  
  
2.  依次展开 **“数据库”**、数据库和 **“表”**。  
  
3.  展开包含该列的表，然后展开 **“列”**。  
  
4.  右键单击该列并选择“属性”。 如果排序规则属性为空，则该列不是字符数据类型。  
  
###  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
 **查看服务器的排序规则设置**  
  
1.  在对象资源管理器中，连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 实例，并在工具栏中单击 **“新建查询”**。  
  
2.  在查询窗口中，输入以下使用 SERVERPROPERTY 系统函数的语句。  
  
    ```sql  
    SELECT CONVERT (varchar(256), SERVERPROPERTY('collation'));  
    ```  
  
3.  或者，您可以使用 sp_helpsort 系统存储过程。  
  
    ```sql  
    EXECUTE sp_helpsort;  
    ```  
  
 **查看 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]**  
  
1.  在对象资源管理器中，连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 实例，并在工具栏中单击 **“新建查询”**。  
  
2.  在查询窗口中，输入以下使用 SERVERPROPERTY 系统函数的语句。  
  
    ```sql  
    SELECT name, description FROM sys.fn_helpcollations();  
    ```  
  
 **查看数据库的排序规则设置**  
  
1.  在对象资源管理器中，连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 实例，并在工具栏中单击 **“新建查询”**。  
  
2.  在查询窗口中，输入以下使用 sys.databases 系统目录视图的语句。  
  
    ```sql  
    SELECT name, collation_name FROM sys.databases;  
    ```  
  
3.  或者，您可以使用 DATABASEPROPERTYEX 的系统函数。  
  
    ```sql  
    SELECT CONVERT (varchar(256), DATABASEPROPERTYEX('database_name','collation'));  
    ```  
  
 **查看列的排序规则设置**  
  
1.  在对象资源管理器中，连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 实例，并在工具栏中单击 **“新建查询”**。  
  
2.  在查询窗口中，输入以下使用 sys.columns 系统目录视图的语句。  
  
    ```sql  
    SELECT name, collation_name FROM sys.columns WHERE name = N'<insert character data type column name>';  
    ```  
  
 **查看表和列的排序规则设置的具体步骤**  

1.  在对象资源管理器中，连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 实例，并在工具栏中单击 **“新建查询”**。  
  
2.  在查询窗口中，输入以下使用 sys.columns 系统目录视图的语句。  
  
    ```sql  
    SELECT t.name TableName, c.name ColumnName, collation_name  
    FROM sys.columns c  
    inner join sys.tables t on c.object_id = t.object_id;  
    ```  



## <a name="see-also"></a>另请参阅  
 [SERVERPROPERTY (Transact-SQL)](../../t-sql/functions/serverproperty-transact-sql.md)   
 [sys.fn_helpcollations (Transact-SQL)](../../relational-databases/system-functions/sys-fn-helpcollations-transact-sql.md)   
 [sys.databases (Transact-SQL)](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)   
 [sys.columns (Transact-SQL)](../../relational-databases/system-catalog-views/sys-columns-transact-sql.md)   
 [排序规则优先级 (Transact-SQL)](../../t-sql/statements/collation-precedence-transact-sql.md)   
 [排序规则和 Unicode 支持](../../relational-databases/collations/collation-and-unicode-support.md)      
 [sp_helpsort (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-helpsort-transact-sql.md)  
  
  
