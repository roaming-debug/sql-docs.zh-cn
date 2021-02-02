---
title: 启用或禁用计划指南 | Microsoft Docs
description: 了解如何使用 SQL Server Management Studio 或 Transact-SQL 来禁用和启用计划指南。 禁用或启用数据库中的一个或所有计划指南。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- plan guides [SQL Server], disabling
- enabling plan guides
- plan guides [SQL Server], enabling
- disabling plan guides
ms.assetid: b00ab550-5308-4cb8-8330-483cd1d25654
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: e6fd86e44411fafef9e70e8981d0fa8728af9e76
ms.sourcegitcommit: 04d101fa6a85618b8bc56c68b9c006b12147dbb5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99048931"
---
# <a name="enable-or-disable-a-plan-guide"></a>启用或禁用计划指南
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  您可以使用 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 或 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 禁用和启用 [!INCLUDE[tsql](../../includes/tsql-md.md)]中的计划指南。 无论是数据库中的单个计划指南还是所有计划指南都可以启用或禁用。  
  
 **本主题内容**  
  
-   **开始之前：**  
  
     [限制和局限](#Restrictions)  
  
     [安全性](#Security)  
  
-   **若要禁用和启用计划指南，请使用：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> 限制和局限  
  
-   如果尝试删除或修改的函数、存储过程或 DML 触发器由某个计划指南引用，则不管该指南为启用状态还是禁用状态，都会导致错误。 在删除或修改以上所列任何对象之前总是检查依赖关系。  
  
-   禁用一个已禁用的计划指南或启用一个已启用的计划指南将不起作用，且运行时没有错误。  
  
###  <a name="security"></a><a name="Security"></a> Security  
  
####  <a name="permissions"></a><a name="Permissions"></a> 权限  
 禁用或启用 OBJECT 计划指南要求对计划指南引用的对象（例如：函数、存储过程）具有 ALTER 权限。 其他所有计划指南都需要 ALTER DATABASE 权限。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
  
#### <a name="to-disable-or-enable-a-plan-guide"></a>禁用或启用计划指南  
  
1.  单击加号以便展开您要禁用或启用计划指南的数据库，然后单击加号以便展开 **“可编程性”** 文件夹。  
  
2.  单击加号以便展开 **“计划指南”** 文件夹。  
  
3.  右键单击要禁用或启用的计划指南，然后选择“禁用”或“启用”。  
  
4.  在 **“禁用计划指南”** 或 **“启用计划指南”** 对话框中，验证所选操作已经成功，然后单击 **“关闭”** 。  

#### <a name="to-disable-or-enable-all-plan-guides-in-a-database"></a>禁用或启用数据库中的所有计划指南  
  
1.  单击加号以便展开您要禁用或启用计划指南的数据库，然后单击加号以便展开 **“可编程性”** 文件夹。  
  
2.  右键单击“计划指南”文件夹，然后选择“全部启用”或“全部禁用”。  
  
3.  在 **“禁用所有计划指南”** 或 **“启用所有计划指南”** 对话框中，验证所选操作已经成功，然后单击 **“关闭”** 。  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
  
#### <a name="to-disable-or-enable-a-plan-guide"></a>禁用或启用计划指南  
  
1.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]的实例。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行” 。  
  
    ```  
    --Create a procedure on which to define the plan guide.  
    IF OBJECT_ID(N'Sales.GetSalesOrderByCountry', N'P') IS NOT NULL  
        DROP PROCEDURE Sales.GetSalesOrderByCountry;  
    GO  
    CREATE PROCEDURE Sales.GetSalesOrderByCountry   
        (@Country nvarchar(60))  
    AS  
    BEGIN  
        SELECT *  
        FROM Sales.SalesOrderHeader AS h   
        INNER JOIN Sales.Customer AS c ON h.CustomerID = c.CustomerID  
        INNER JOIN Sales.SalesTerritory AS t ON c.TerritoryID = t.TerritoryID  
        WHERE t.CountryRegionCode = @Country;  
    END  
    GO  
    --Create the plan guide.  
    EXEC sp_create_plan_guide N'Guide3',  
        N'SELECT *  
        FROM Sales.SalesOrderHeader AS h   
        INNER JOIN Sales.Customer AS c ON h.CustomerID = c.CustomerID  
        INNER JOIN Sales.SalesTerritory AS t ON c.TerritoryID = t.TerritoryID  
        WHERE t.CountryRegionCode = @Country',  
        N'OBJECT',  
        N'Sales.GetSalesOrderByCountry',  
        NULL,  
        N'OPTION (OPTIMIZE FOR (@Country = N''US''))';  
    --Disable the plan guide.  
    EXEC sp_control_plan_guide N'DISABLE', N'Guide3';  
    GO  
    --Enable the plan guide.  
    EXEC sp_control_plan_guide N'ENABLE', N'Guide3';  
    GO  
  
    ```  
  
#### <a name="to-disable-or-enable-all-plan-guides-in-a-database"></a>禁用或启用数据库中的所有计划指南  
  
1.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]的实例。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行” 。  
  
    ```  
    --Disable all plan guides in the database.  
    EXEC sp_control_plan_guide N'DISABLE ALL';  
    GO  
    --Enable all plan guides in the database.  
    EXEC sp_control_plan_guide N'ENABLE ALL';  
    GO  
  
    ```  
  
 有关详细信息，请参阅 [sp_control_plan_guide (Transact SQL)](../../relational-databases/system-stored-procedures/sp-control-plan-guide-transact-sql.md)。  
  
  
