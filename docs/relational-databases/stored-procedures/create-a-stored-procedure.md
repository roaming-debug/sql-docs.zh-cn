---
title: 创建存储过程 | Microsoft Docs
description: 了解如何使用 SQL Server Management Studio 和 Transact-SQL CREATE PROCEDURE 语句来创建 Transact-SQL 存储过程。
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: stored-procedures
ms.topic: quickstart
helpviewer_keywords:
- new stored procedures
- stored procedures [SQL Server], creating
- creating stored procedures
ms.assetid: 76e8a6ba-1381-4620-b356-4311e1331ca7
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: c1ab9e2681f229fba5294f061d8d83e98aabac64
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97475318"
---
# <a name="create-a-stored-procedure"></a>创建存储过程
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]


本主题介绍了如何通过使用 [!INCLUDE[tsql](../../includes/tsql-md.md)] 和 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] CREATE PROCEDURE 语句来创建 [!INCLUDE[tsql](../../includes/tsql-md.md)] 存储过程。  
  
-   **开始之前：** [权限](#Permissions)  
  
-   **要创建该过程，请使用：** [SQL Server Management Studio](#SSMSProcedure)、[Transact-SQL](#TsqlProcedure)  
  
##  <a name="permissions"></a><a name="Permissions"></a> 权限  
 需要在数据库中有 CREATE PROCEDURE 权限，对在其中创建过程的架构有 ALTER 权限。  
  
##  <a name="how-to-create-a-stored-procedure"></a><a name="Procedures"></a> 如何创建存储过程  
 您可以使用以下项之一：  
  
-   [SQL Server Management Studio](#SSMSProcedure)  
  
-   [Transact-SQL](#TsqlProcedure)  
  
###  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
 **在对象资源管理器中创建过程**  
  
1.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 的实例，然后展开该实例。  
  
2.  依次展开 **“数据库”** 、 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库和 **“可编程性”** 。  
  
3.  右键单击“存储过程”，再单击“新建存储过程”。  
  
4.  在 **“查询”** 菜单上，单击 **“指定模板参数的值”** 。  
  
5.  在 **“指定模板参数的值”** 对话框中，输入下列所示的参数值。  
  
    |参数|值|  
    |---------------|-----------|  
    |作者|*您的姓名*|  
    |创建日期|*今天的日期*|  
    |说明|返回雇员数据。|  
    |Procedure_name|HumanResources.uspGetEmployeesTest|  
    |@Param1|@LastName|  
    |@Datatype_For_Param1|**nvarchar**(50)|  
    |Default_Value_For_Param1|Null|  
    |@Param2|@FirstName|  
    |@Datatype_For_Param2|**nvarchar**(50)|  
    |Default_Value_For_Param2|Null|  
  
6.  单击“确定”。  
  
7.  在 **“查询编辑器”** 中，使用以下语句替换 SELECT 语句：  
  
    ```sql  
    SELECT FirstName, LastName, Department  
    FROM HumanResources.vEmployeeDepartmentHistory  
    WHERE FirstName = @FirstName AND LastName = @LastName  
        AND EndDate IS NULL;  
    ```  
  
8.  若要测试语法，请在 **“查询”** 菜单上，单击 **“分析”** 。 如果返回错误消息，则请将这些语句与上述信息进行比较，并视需要进行更正。  
  
9. 若要创建该过程，请在 **“查询”** 菜单上单击 **“执行”** 。 该过程作为数据库中的对象创建。  
  
10. 若要查看在对象资源管理器中列出的过程，请右键单击“存储过程”，然后选择“刷新”。  
  
11. 若要运行该过程，请在对象资源管理器中右键单击存储过程名称 **HumanResources.uspGetEmployeesTest**，然后选择“执行存储过程”。  
  
12. 在“执行过程”窗口中，输入 Margheim 作为参数 @LastName 的值，并输入值 Diane 作为参数 @FirstName 的值。  
  
> [!WARNING]  
>  验证所有用户的输入。 验证前请勿连接用户输入。 绝对不要执行根据尚未验证的用户输入构造的命令。  
  
###  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
 **在查询编辑器中创建过程**  
  
1.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]的实例。  
  
2.  从 **“文件”** 菜单中，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行” 。 该示例将使用其他过程名称创建与上述相同的存储过程。  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    CREATE PROCEDURE HumanResources.uspGetEmployeesTest2   
        @LastName nvarchar(50),   
        @FirstName nvarchar(50)   
    AS   
  
        SET NOCOUNT ON;  
        SELECT FirstName, LastName, Department  
        FROM HumanResources.vEmployeeDepartmentHistory  
        WHERE FirstName = @FirstName AND LastName = @LastName  
        AND EndDate IS NULL;  
    GO  
  
    ```  
  
4.  若要运行该过程，请将以下示例复制并粘贴到一个新的查询窗口中，然后单击 **“执行”** 。 请注意，将显示指定参数值的不同方法。  
  
    ```  
    EXECUTE HumanResources.uspGetEmployeesTest2 N'Ackerman', N'Pilar';  
    -- Or  
    EXEC HumanResources.uspGetEmployeesTest2 @LastName = N'Ackerman', @FirstName = N'Pilar';  
    GO  
    -- Or  
    EXECUTE HumanResources.uspGetEmployeesTest2 @FirstName = N'Pilar', @LastName = N'Ackerman';  
    GO  
  
    ```  
  
## <a name="see-also"></a>另请参阅  
 [CREATE PROCEDURE (Transact-SQL)](../../t-sql/statements/create-procedure-transact-sql.md)  
  
  
