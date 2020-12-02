---
description: 创建同义词
title: 创建同义词 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.technology: t-sql
ms.topic: conceptual
f1_keywords:
- sql13.swb.synonym.general.f1
helpviewer_keywords:
- creating synonyms
- synonyms [SQL Server], creating
ms.assetid: fedfa7a5-d0b6-4e2b-90f4-a08122958e33
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: f5c4d25ed48715c9317ebe0df37ee4d2779abe10
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96125106"
---
# <a name="create-synonyms"></a>创建同义词
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  本主题说明如何使用 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 或 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 在 [!INCLUDE[tsql](../../includes/tsql-md.md)]中创建同义词。  
  
 **本主题内容**  
  
-   **开始之前：**  
  
     [安全性](#Security)  
  
-   **若要创建同义词，可使用：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="security"></a><a name="Security"></a> Security  
 若要在给定架构中创建同义词，则用户必须具有 CREATE SYNONYM 权限，并拥有架构或具有 ALTER SCHEMA 权限。 CREATE SYNONYM 权限是可授予的权限。  
  
####  <a name="permissions"></a><a name="Permissions"></a> 权限  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
  
#### <a name="to-create-a-synonym"></a>创建同义词  
  
1.  在 **“对象资源管理器”** 中，展开要创建新视图的数据库。  
  
2.  右键单击“同义词”文件夹，然后单击“新建同义词...”。  
  
3.  在 **“添加同义词”** 对话框中，输入以下信息。  

     **同义词名称**  
     键入将用于此对象的新名称。  
  
     **同义词架构**  
     键入将用于此对象的新名称的架构。  
  
     **服务器名称**  
     键入要连接到的服务器实例。  
  
     **数据库名称**  
     键入或选择包含该对象的数据库。  
  
     **架构**  
     键入或选择该对象所属的架构。  
  
     **对象类型**  
     选择对象的类型。  
  
     **对象名称**  
     键入同义词所引用的对象的名称。  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
  
#### <a name="to-create-a-synonym"></a>创建同义词  
  
1.  连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击 **“执行”** 。  
  
###  <a name="example-transact-sql"></a><a name="TsqlExample"></a> 示例 (Transact-SQL)  
 下面的示例为 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库中的现有表创建一个同义词。 后续示例中将使用该同义词。  
  
```  
USE tempdb;  
GO  
CREATE SYNONYM MyAddressType  
FOR AdventureWorks2012.Person.AddressType;  
GO  
```  
  
 以下示例将行插入到由 `MyAddressType` 同义词引用的基表。  
  
```  
USE tempdb;  
GO  
INSERT INTO MyAddressType (Name)  
VALUES ('Test');  
GO  
```  
  
 以下示例说明了如何在动态 SQL 中引用同义词。  
  
```  
USE tempdb;  
GO  
EXECUTE ('SELECT Name FROM MyAddressType');  
GO  
```  
  
  
