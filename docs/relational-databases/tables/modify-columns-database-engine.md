---
description: 修改列（数据库引擎）
title: 修改列（数据库引擎）| Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: table-view-index, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- modifying data types
- column data types [SQL Server]
- data types [SQL Server], columns
ms.assetid: b67b95c5-61ef-4bd8-9a3e-1640eb7583ac
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 0f306dd339ed7e91a1d54195dc32346c9139c809
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88460209"
---
# <a name="modify-columns-database-engine"></a>修改列（数据库引擎）
[!INCLUDE [sqlserver2016-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa-pdw.md)]

  您可以通过使用 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 或 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 在 [!INCLUDE[tsql](../../includes/tsql-md.md)]中修改列的数据类型。  
  
> [!WARNING]  
>  如果修改已包含数据的列的数据类型，则在将现有数据转换为新类型时可能会导致永久丢失数据。 此外，依赖于所修改列的代码和应用程序可能会失败。 这些代码和应用程序包括查询、视图、存储过程、用户定义函数和客户端应用程序等。 注意，这些错误会级联发生。 例如，如果一个存储过程调用一个依赖于所修改列的用户定义函数，则该存储过程可能会失败。 请在需要对列进行任何更改之前慎重考虑。  
  
 **本主题内容**  
  
-   **开始之前：**  
  
     [安全性](#Security)  
  
-   **若要修改列的数据类型，请使用：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="security"></a><a name="Security"></a> Security  
  
####  <a name="permissions"></a><a name="Permissions"></a> 权限  
 需要对表的 ALTER 权限。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
  
#### <a name="to-modify-the-data-type-of-a-column"></a>修改列的数据类型  
  
1.  在“对象资源管理器”中，右键单击要更改其小数位数的列所在的表，再单击“设计”。  
  
2.  选择要修改其数据类型的列。  
  
3.  在“列属性”选项卡中，单击“数据类型”属性的网格单元格，再从下拉列表中选择新的数据类型。  
  
4.  在“文件”菜单上，单击“保存表名称”。  
  
> [!NOTE]  
>  当您修改列的数据类型时，即使已为所选数据类型指定其他长度，表设计器也会使用该数据类型的默认长度。 在指定数据类型之后，始终需要将数据类型长度设置为所需的值。  
  
> [!WARNING]  
>  如果您尝试修改与其他表相关的列的数据类型，表设计器会要求您确认也应该对其他表中的列进行更改。  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
  
#### <a name="to-modify-the-data-type-of-a-column"></a>修改列的数据类型  
  
1.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]的实例。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行” 。  
  
    ```sql  
    CREATE TABLE dbo.doc_exy (column_a INT ) ;  
    GO  
    INSERT INTO dbo.doc_exy (column_a) VALUES (10) ;  
    GO  
    ALTER TABLE dbo.doc_exy ALTER COLUMN column_a DECIMAL (5, 2) ;  
    GO  
  
    ```  
  
 有关详细信息，请参阅 [ALTER TABLE (Transact-SQL)](../../t-sql/statements/alter-table-transact-sql.md)  
  
  
