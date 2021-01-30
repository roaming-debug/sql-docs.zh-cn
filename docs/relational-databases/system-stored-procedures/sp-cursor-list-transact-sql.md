---
description: sp_cursor_list (Transact-SQL)
title: sp_cursor_list (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_cursor_list
- sp_cursor_list_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_cursor_list
ms.assetid: 7187cfbe-d4d9-4cfa-a3bb-96a544c7c883
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 3a60d447d7072c6386faef08d1dd7398f906064d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205190"
---
# <a name="sp_cursor_list-transact-sql"></a>sp_cursor_list (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  报告当前为连接打开的服务器游标的属性。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_cursor_list [ @cursor_return = ] cursor_variable_name OUTPUT   
     , [ @cursor_scope = ] cursor_scope  
[;]  
```  
  
## <a name="arguments"></a>参数  
 [ @cursor_return =] *cursor_variable_name* 输出  
 已声明的游标变量的名称。 *cursor_variable_name* 是 **游标**，无默认值。 游标是可滚动、动态的只读游标。  
  
 [ @cursor_scope =] *cursor_scope*  
 指定要报告的游标级别。 *cursor_scope* 是 **int**，没有默认值，可以是下列值之一。  
  
|值|说明|  
|-----------|-----------------|  
|1|报告所有本地游标。|  
|2|报告所有全局游标。|  
|3|报告本地游标和全局游标。|  
  
## <a name="return-code-values"></a>返回代码值  
 无  
  
## <a name="cursors-returned"></a>返回的游标  
 sp_cursor_list 返回的报告是 [!INCLUDE[tsql](../../includes/tsql-md.md)] 游标输出参数，而不是结果集。 这样，[!INCLUDE[tsql](../../includes/tsql-md.md)] 批处理、存储过程和触发器便可以按一次一行的方式处理输出。 这还意味着无法直接从数据库 API 函数调用该过程。 游标输出参数必须绑定到程序变量，但是数据库 API 不支持绑定游标参数或变量。  
  
 以下是 sp_cursor_list 返回的游标格式。 游标格式与 sp_describe_cursor 返回的格式相同。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|reference_name|**sysname**|用于引用游标的名称。 如果通过 DECLARE CURSOR 语句中给定的名称引用游标，则引用名称与游标名称相同。 如果通过变量引用游标，则引用名称为游标变量的名称。|  
|cursor_name|**sysname**|来自 DECLARE CURSOR 语句的游标名称。 在中 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，如果游标是通过将游标变量设置为游标创建的， **cursor_name** 将返回游标变量的名称。  在早期版本中，此输出列将返回系统生成的名称。|  
|cursor_scope|**smallint**|1 = LOCAL<br /><br /> 2 = GLOBAL|  
|status|**smallint**|与 CURSOR_STATUS 系统函数报告的值相同：<br /><br /> 1 = 游标名称或游标变量引用的游标为打开状态。 如果游标是不敏感的、静态的或是键集，则至少具有一行。 如果游标是动态的，则结果集具有零行或多行。<br /><br /> 0 = 游标名称或游标变量引用的游标为打开状态，但不包含任何行。 动态游标从不返回此值。<br /><br /> -1 = 游标名称或游标变量引用的游标为关闭状态。<br /><br /> -2 = 仅适用于游标变量。 没有为该变量分配任何游标。 这可能是由于某个 OUTPUT 参数为该变量分配了游标，但存储过程在返回前关闭了游标。<br /><br /> -3 = 指定名称的游标或游标变量不存在，或没有为该游标变量分配游标。|  
|模型|**smallint**|1 = 不敏感（或静态）<br /><br /> 2 = 键集<br /><br /> 3 = 动态<br /><br /> 4 = 快进|  
|concurrency|**smallint**|1 = 只读<br /><br /> 2 = 滚动锁<br /><br /> 3 = 乐观|  
|scrollable|**smallint**|0 = 只进<br /><br /> 1 = 可滚动|  
|open_status|**smallint**|0 = 关闭的<br /><br /> 1 = 打开的|  
|cursor_rows|**int**|结果集中合格的行数。 有关详细信息，请[参阅 @CURSOR_ROWS @](../../t-sql/functions/cursor-rows-transact-sql.md)。|  
|fetch_status|**smallint**|此游标上次提取的状态。 有关详细信息，请[参阅 @FETCH_STATUS @](../../t-sql/functions/fetch-status-transact-sql.md)：<br /><br /> 0 = 提取成功。<br /><br /> -1 = 提取失败或超出游标的界限。<br /><br /> -2 = 缺少所请求的行。<br /><br /> -9 = 尚未对游标进行提取。|  
|column_count|**smallint**|游标结果集中的列数。|  
|row_count|**smallint**|上次对游标的操作所影响的行数。 有关详细信息，请[参阅 @ROWCOUNT @](../../t-sql/functions/rowcount-transact-sql.md)。|  
|last_operation|**smallint**|上次对游标执行的操作：<br /><br /> 0 = 没有对游标执行操作。<br /><br /> 1 = OPEN<br /><br /> 2 = FETCH<br /><br /> 3 = 插入<br /><br /> 4 = UPDATE<br /><br /> 5 = DELETE<br /><br /> 6 = CLOSE<br /><br /> 7 = DEALLOCATE|  
|cursor_handle|**int**|在服务器作用域内标识游标的唯一值。|  
  
## <a name="remarks"></a>备注  
 sp_cursor_list 生成连接打开的当前服务器游标列表，并对每个游标的全局属性进行说明，例如游标的可滚动性和可更新性。 sp_cursor_list 列出的游标包括：  
  
-   [!INCLUDE[tsql](../../includes/tsql-md.md)] 服务器游标。  
  
-   API 服务器游标，由 ODBC 应用程序打开，然后调用 SQLSetCursorName 为游标命名。  
  
 使用 sp_describe_cursor_columns 对游标返回的结果集的属性进行说明。 使用 sp_describe_cursor_tables 报告游标引用的基表。 sp_describe_cursor 报告的信息与 sp_cursor_list 相同，但只用于指定的游标。  
  
## <a name="permissions"></a>权限  
 执行权限默认授予 public 角色。  
  
## <a name="examples"></a>示例  
 下面的示例将打开一个全局游标，并使用 `sp_cursor_list` 报告该游标的属性。  
  
```  
USE AdventureWorks2012;  
GO  
-- Declare and open a keyset-driven cursor.  
DECLARE abc CURSOR KEYSET FOR  
SELECT LastName  
FROM Person.Person  
WHERE LastName LIKE 'S%';  
OPEN abc;  
  
-- Declare a cursor variable to hold the cursor output variable  
-- from sp_cursor_list.  
DECLARE @Report CURSOR;  
  
-- Execute sp_cursor_list into the cursor variable.  
EXEC master.dbo.sp_cursor_list @cursor_return = @Report OUTPUT,  
      @cursor_scope = 2;  
  
-- Fetch all the rows from the sp_cursor_list output cursor.  
FETCH NEXT from @Report;  
WHILE (@@FETCH_STATUS <> -1)  
BEGIN  
   FETCH NEXT from @Report;  
END  
  
-- Close and deallocate the cursor from sp_cursor_list.  
CLOSE @Report;  
DEALLOCATE @Report;  
GO  
  
-- Close and deallocate the original cursor.  
CLOSE abc;  
DEALLOCATE abc;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
