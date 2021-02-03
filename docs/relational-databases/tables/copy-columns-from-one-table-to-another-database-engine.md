---
description: 将列从一个表复制到另一个表 (数据库引擎)
title: 将列从一个表复制到另一个表 (数据库引擎) | Microsoft Docs
ms.custom: ''
ms.date: 09/01/2016
ms.prod: sql
ms.prod_service: table-view-index, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- copying columns
- columns [SQL Server], copying
ms.assetid: 5f5e70dc-69f9-44b8-bc48-b5d51ac20d77
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 46341304bf8f05e53a4143e8c5a151d2f9023155
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181494"
---
# <a name="copy-columns-from-one-table-to-another-database-engine"></a>将列从一个表复制到另一个表 (数据库引擎)
[!INCLUDE [sqlserver2016-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa-pdw.md)]

  本主题介绍如何使用 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 或 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 在 [!INCLUDE[tsql](../../includes/tsql-md.md)]中通过仅复制列定义或者复制定义和数据将列从一个表复制到另一个表。  
  
 **本主题内容**  
  
-   **开始之前：**  
  
     [限制和局限](#Restrictions)  
  
     [安全性](#Security)  
  
-   **若要复制列，请使用：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> 限制和局限  
 当将具有别名数据类型的列从一个数据库复制到另一个数据库时，别名数据类型在目标数据库中可能不可用。 在这种情况下，将为该列分配该数据库中可用的匹配度最高的基数据类型。  
  
###  <a name="security"></a><a name="Security"></a> Security  
  
####  <a name="permissions"></a><a name="Permissions"></a> 权限  
 需要对表的 ALTER 权限。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
  
#### <a name="to-copy-column-definitions-from-one-table-to-another"></a>将列定义从一个表复制到另一个表  
  
1.  右键单击相应表，然后单击“设计”，打开要复制的列所在的表以及要复制到的表。  
  
2.  单击包含要复制的列的表的选项卡，然后选择这些列。  
  
3.  在 **“编辑”** 菜单中，单击 **“复制”**。  
  
4.  单击要将列复制到的表的选项卡。  
  
5.  选择要排在插入列之后的列，然后在 **“编辑”** 菜单中，单击 **“粘贴”**。  

#### <a name="to-copy-data-from-one-table-to-another"></a>将数据从一个表复制到另一个表  
  
1.  按照以上关于复制列定义的说明执行操作。  
  
    > [!NOTE]  
    >  在开始将数据从一个表复制到另一个表之前，请确保目标列中的数据类型与源列的数据类型相兼容。  
  
2.  打开一个新的查询编辑器窗口。 

3.  右键单击查询编辑器，然后单击“在编辑器中设计查询”。 

4.  在“添加表”对话框中，选择源和目标表，单击“添加”，然后关闭“添加表”对话框。 

5.  右键单击查询编辑器的打开区域，指向“更改类型”，然后单击“插入结果”。  

6.  在“选择插入结果的目标表”对话框中，选择目标表。 

7.  在查询设计器的上半部分，单击源表中的源列。

8. 查询设计器现在已创建了 INSERT 查询。 单击“确定”以将该查询置于原始查询编辑器窗口中。  

9.  执行该查询以将数据从源表插入目标表。

  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
  
#### <a name="to-copy-column-definitions-from-one-table-to-another"></a>将列定义从一个表复制到另一个表  
  
1.  不能通过使用 Transact-SQL 语句将单独列从一个表复制到另一个现有表。 但是，您可以通过使用 SELECT INTO 在默认文件组中创建一个新表，并将来自查询的结果行插入该表中。 有关详细信息，请参阅 [ INTO 子句 (Transact-SQL)](../../t-sql/queries/select-into-clause-transact-sql.md)。  
  
#### <a name="to-copy-data-from-one-table-to-another"></a>将数据从一个表复制到另一个表  
  
1.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]的实例。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行”  。  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    CREATE TABLE dbo.EmployeeSales  
    ( BusinessEntityID   varchar(11) NOT NULL,  
      SalesYTD money NOT NULL  
    );  
    GO  
    INSERT INTO dbo.EmployeeSales  
        SELECT BusinessEntityID, SalesYTD   
        FROM Sales.SalesPerson;  
    GO  
    ```  
  
  
