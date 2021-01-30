---
description: sp_get_query_template (Transact-SQL)
title: sp_get_query_template (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_get_query_template
- sp_get_query_template_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_get_query_template
ms.assetid: 85e9bef7-2417-41a8-befa-fe75507d9bf2
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 89e30950c072cbfe5737a65934155276bbf6a27d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204775"
---
# <a name="sp_get_query_template-transact-sql"></a>sp_get_query_template (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  返回参数化格式的查询。 返回的结果模拟使用强制参数化得到的参数化格式的查询。 sp_get_query_template 主要在创建 TEMPLATE 计划指南时使用。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_get_query_template  
   [ @querytext = ] N'query_text'  
   , @templatetext OUTPUT   
   , @parameters OUTPUT   
```  
  
## <a name="arguments"></a>参数  
 "*query_text*"  
 要生成参数化版本的查询。 "*query_text*" 必须用单引号引起来，并以 N Unicode 说明符开头。 N "*query_text*" 是分配给参数的值 @querytext 。 这种类型的类型为 **nvarchar (max)**。  
  
 @templatetext  
 类型为 **nvarchar (max)** 的输出参数，以字符串文字形式接收 *query_text* 的参数化形式。  
  
 @parameters  
 是 nvarchar 类型的输出参数 **(max)**，按指示提供，用于接收参数名称和已参数化的数据类型的字符串文字 @templatetext 。  
  
## <a name="remarks"></a>备注  
 如果出现下列情况，sp_get_query_template 会返回错误。  
  
-   它不会在 *query_text* 中参数化任何常量文本值。  
  
-   *query_text* 为 NULL，而不是 Unicode 字符串、语法无效或无法编译。  
  
 如果 sp_get_query_template 返回错误，则不会修改 @templatetext 和 @parameters 输出参数的值。  
  
## <a name="permissions"></a>权限  
 要求具有公用数据库角色的成员身份。  
  
## <a name="examples"></a>示例  
 以下示例返回包含两个常量文字值的参数化格式的查询。  
  
```  
USE AdventureWorks2012;  
GO  
DECLARE @my_templatetext nvarchar(max)  
DECLARE @my_parameters nvarchar(max)  
EXEC sp_get_query_template   
    N'SELECT pi.ProductID, SUM(pi.Quantity) AS Total  
        FROM Production.ProductModel pm   
        INNER JOIN Production.ProductInventory pi  
        ON pm.ProductModelID = pi.ProductID  
        WHERE pi.ProductID = 2  
        GROUP BY pi.ProductID, pi.Quantity  
        HAVING SUM(pi.Quantity) > 400',  
@my_templatetext OUTPUT,  
@my_parameters OUTPUT;  
SELECT @my_templatetext;  
SELECT @my_parameters;  
```  
  
 下面是 `@my_templatetext``OUTPUT` 参数的参数化结果：  
  
 `select pi . ProductID , SUM ( pi . Quantity ) as Total`  
  
 `from Production . ProductModel pm`  
  
 `inner join Production . ProductInventory pi`  
  
 `on pm . ProductModelID = pi . ProductID`  
  
 `where pi . ProductID = @0`  
  
 `group by pi . ProductID , pi . Quantity`  
  
 `having SUM ( pi . Quantity ) > 400`  
  
 请注意，第一个常量文字 `2` 将转换为参数。 第二个文字 `400` 未转换，因为它包含在 `HAVING` 子句中。 如果 ALTER DATABASE 的 PARAMETERIZATION 选项设置为 FORCED，sp_get_query_template 返回的结果将模拟参数化格式的查询。  
  
 下面是 `@my_parameters OUTPUT` 参数的参数化结果：  
  
```  
@0 int  
```  
  
> [!NOTE]  
>  在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的各个快速修复工程、Service Pack 和版本升级之间，sp_get_query_template 输出中参数的顺序和命名会发生变化。 另外，升级会导致同一查询的不同的常量文字集被参数化，并且对两种输出参数的结果应用不同的文本间距。  
  
## <a name="see-also"></a>另请参阅  
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [数据库引擎存储过程 &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [使用计划指南指定查询参数化行为](../../relational-databases/performance/specify-query-parameterization-behavior-by-using-plan-guides.md)  
  
  
