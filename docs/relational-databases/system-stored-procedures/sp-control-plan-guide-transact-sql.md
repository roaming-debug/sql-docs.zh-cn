---
description: sp_control_plan_guide (Transact-SQL)
title: sp_control_plan_guide (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_control_plan_guide
- sp_control_plan_guide_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_control_plan_guide
ms.assetid: c96d43d5-6507-4d66-b3f5-f44c0617cb5c
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 0b4f5d1adbdadf9d8291257708a3098fb50ce175
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185786"
---
# <a name="sp_control_plan_guide-transact-sql"></a>sp_control_plan_guide (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  删除、启用或禁用计划指南。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_control_plan_guide [ @operation = ] N'<control_option>'  
  [ , [ @name = ] N'plan_guide_name' ]  
  
<control_option>::=  
{   
    DROP   
  | DROP ALL  
  | DISABLE  
  | DISABLE ALL  
  | ENABLE   
  | ENABLE ALL  
}  
```  
  
## <a name="arguments"></a>参数  
 **N '** _plan_guide_name_ **'**  
 指定被删除、启用或禁用的计划指南。 *plan_guide_name* 解析为当前数据库。 如果未指定， *plan_guide_name* 默认为 NULL。  
  
 DROP  
 删除 *plan_guide_name* 指定的计划指南。 删除计划指南后，以后再执行以前与该计划指南匹配的查询时将不受该计划指南的影响。  
  
 DROP ALL  
 删除当前数据库中的所有计划指南。 当指定 DROP ALL 时，不能指定 **N '**_plan_guide_name_ 。  
  
 DISABLE  
 禁用 *plan_guide_name* 指定的计划指南。 禁用计划指南后，以后再执行以前与该计划指南匹配的查询时将不受该计划指南的影响。  
  
 DISABLE ALL  
 禁用当前数据库中的所有计划指南。 指定 "禁用 ALL" 时，不能指定 **N '**_plan_guide_name_ 。  
  
 ENABLE  
 启用 *plan_guide_name* 指定的计划指南。 启用计划指南后，可以使其与合格查询匹配。 默认情况下，计划指南在创建时启用。  
  
 ENABLE ALL  
 启用当前数据库中的所有计划指南。 指定了 ENABLE ALL 时，不能指定 **N '**_plan_guide_name_**'**。  
  
## <a name="remarks"></a>备注  
 如果尝试删除或修改的函数、存储过程或 DML 触发器由某个计划指南引用，则不管该指南为启用状态还是禁用状态，都会导致错误。  
  
 禁用一个已禁用的计划指南或启用一个已启用的计划指南将不起作用，且运行时没有错误。  
  
 并非在的每个版本中都提供计划指南 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 各版本支持的功能列表，请参阅 [SQL Server 2016 的版本和支持的功能](../../sql-server/editions-and-components-of-sql-server-2016.md)。 不过，您可以在的任何版本中，用 DROP 或 DROP ALL 选项执行 **sp_control_plan_guide** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
## <a name="permissions"></a>权限  
 若要在类型为 object 的计划指南 (创建指定 **@type 为 = '** object ) **'** 的计划指南的 **sp_control_plan_guide** ，则需要对该计划指南引用的对象具有 ALTER 权限。 其他所有计划指南都需要 ALTER DATABASE 权限。  
  
## <a name="examples"></a>示例  
  
### <a name="a-enabling-disabling-and-dropping-a-plan-guide"></a>A. 启用、禁用和删除计划指南  
 以下示例创建一个计划指南，然后禁用、启用该计划指南并将其删除。  
  
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
GO  
--Disable the plan guide.  
EXEC sp_control_plan_guide N'DISABLE', N'Guide3';  
GO  
--Enable the plan guide.  
EXEC sp_control_plan_guide N'ENABLE', N'Guide3';  
GO  
--Drop the plan guide.  
EXEC sp_control_plan_guide N'DROP', N'Guide3';  
```  
  
### <a name="b-disabling-all-plan-guides-in-the-current-database"></a>B. 禁用当前数据库中的所有计划指南  
 以下示例禁用 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库中的所有计划指南。  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_control_plan_guide N'DISABLE ALL';  
```  
  
## <a name="see-also"></a>另请参阅  
 [数据库引擎存储过程 &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [sp_create_plan_guide (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-create-plan-guide-transact-sql.md)   
 [sys.plan_guides (Transact-SQL)](../../relational-databases/system-catalog-views/sys-plan-guides-transact-sql.md)   
 [计划指南](../../relational-databases/performance/plan-guides.md)  
  
