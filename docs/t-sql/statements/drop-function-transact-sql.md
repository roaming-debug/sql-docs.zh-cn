---
description: DROP FUNCTION (Transact-SQL)
title: DROP FUNCTION (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 02/11/2020
ms.prod: sql
ms.prod_service: sql-data-warehouse, database-engine, pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DROP_FUNCTION_TSQL
- DROP FUNCTION
dev_langs:
- TSQL
helpviewer_keywords:
- user-defined functions [SQL Server], removing
- removing user-defined functions
- DROP FUNCTION statement
- dropping user-defined functions
- deleting user-defined functions
ms.assetid: ee5ad283-9e44-4109-902f-0ce12669ee11
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 655cf93adfd1fbc84d6ab6333fb42e88a66826dd
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98096934"
---
# <a name="drop-function-transact-sql"></a>DROP FUNCTION (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  从当前数据库中删除一个或多个用户定义函数。 用户定义函数使用 [CREATE FUNCTION](../../t-sql/statements/create-function-transact-sql.md) 创建，使用 [ALTER FUNCTION](../../t-sql/statements/alter-function-transact-sql.md) 修改。  
  
 DROP 函数支持本机编译的标量用户定义函数。 有关详细信息，请参阅[内存中 OLTP 的标量用户定义函数](../../relational-databases/in-memory-oltp/scalar-user-defined-functions-for-in-memory-oltp.md)。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
 -- SQL Server, Azure SQL Database 

DROP FUNCTION [ IF EXISTS ] { [ schema_name. ] function_name } [ ,...n ]   
[;]
```

```syntaxsql
 -- Azure Synapse Analytics, Parallel Data Warehouse 

DROP FUNCTION [IF EXISTS] [ schema_name. ] function_name
[;] 
```  
   
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 *IF EXISTS*    
 只有在函数已存在时才对其进行有条件地删除。 在 [!INCLUDE[sssds_md](../../includes/sssds-md.md)] 中以及从 [!INCLUDE[ssnoversion_md](../../includes/ssnoversion-md.md)] 2016 开始可用。
  
 *schema_name*  
 用户定义函数所属的架构的名称。  
  
 function_name  
 要删除的用户定义函数的名称。 可以选择是否指定架构名称。 不能指定服务器名称和数据库名称。  
  
## <a name="remarks"></a>注解  
 如果数据库中存在引用 DROP FUNCTION 的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 函数或视图并且这些函数或视图通过使用 SCHEMABINDING 创建，或者存在引用该函数的计算列、CHECK 约束或 DEFAULT 约束，则 DROP FUNCTION 将失败。  
  
 如果存在引用此函数并且已生成索引的计算列，则 DROP FUNCTION 将失败。  
  
## <a name="permissions"></a>权限  
 若要执行 DROP FUNCTION，用户至少应对函数所属架构具有 ALTER 权限，或对函数具有 CONTROL 权限。  
  
## <a name="examples"></a>示例  
  
### <a name="a-dropping-a-function"></a>A. 删除函数  
 以下示例从 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] 示例数据库的 `Sales` 架构中删除 `fn_SalesByStore` 用户定义函数。 若要创建此函数，请参阅 [CREATE FUNCTION (Transact-SQL)](../../t-sql/statements/create-function-transact-sql.md) 中的示例 B。  
  
```sql  
DROP FUNCTION Sales.fn_SalesByStore;  
```  
  
## <a name="see-also"></a>另请参阅  
 [ALTER FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-function-transact-sql.md)   
 [CREATE FUNCTION (Transact-SQL)](../../t-sql/statements/create-function-transact-sql.md)   
 [OBJECT_ID (Transact-SQL)](../../t-sql/functions/object-id-transact-sql.md)   
 [EVENTDATA (Transact-SQL)](../../t-sql/functions/eventdata-transact-sql.md)   
 [sys.sql_modules (Transact-SQL)](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md)   
 [sys.parameters (Transact-SQL)](../../relational-databases/system-catalog-views/sys-parameters-transact-sql.md)  
  
  
