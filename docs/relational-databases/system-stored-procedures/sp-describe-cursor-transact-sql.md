---
description: sp_describe_cursor (Transact-SQL)
title: sp_describe_cursor (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_describe_cursor
- sp_describe_cursor_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_describe_cursor
ms.assetid: 0c836c99-1147-441e-998c-f0a30cd05275
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 0787f088205aab72c437e22bfac265ef05ccbe19
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200791"
---
# <a name="sp_describe_cursor-transact-sql"></a>sp_describe_cursor (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  报告服务器游标的属性。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_describe_cursor [ @cursor_return = ] output_cursor_variable OUTPUT   
     { [ , [ @cursor_source = ] N'local'  
    , [ @cursor_identity = ] N'local_cursor_name' ]   
   | [ , [ @cursor_source = ] N'global'  
    , [ @cursor_identity = ] N'global_cursor_name' ]   
   | [ , [ @cursor_source = ] N'variable'  
     , [ @cursor_identity = ] N'input_cursor_variable' ]   
     }   
[;]  
```  
  
## <a name="arguments"></a>参数  
 [ @cursor_return =] *output_cursor_variable* 输出  
 声明的用于接收游标输出的游标变量的名称。 *output_cursor_variable* 是 **游标**，无默认值，并且在调用 sp_describe_cursor 时，不能与任何游标相关联。 返回的游标是可滚动的动态只读游标。  
  
 [ @cursor_source =] {N'local ' |N'global ' |N'variable' }  
 指定使用以下哪一名称来指定所报告的游标：本地游标、全局游标或游标变量的名称。 参数 **(30) 为 nvarchar**。  
  
 [ @cursor_identity =] N '*local_cursor_name*']  
 由具有 LOCAL 关键字或默认设置为 LOCAL 的 DECLARE CURSOR 语句创建的游标名称。 *local_cursor_name* **(128) 为 nvarchar**。  
  
 [ @cursor_identity =] N '*global_cursor_name*']  
 由具有 GLOBAL 关键字或默认设置为 GLOBAL 的 DECLARE CURSOR 语句创建的游标名称。 *global_cursor_name* **(128) 为 nvarchar**。  
  
 *global_cursor_name* 还可以是由 ODBC 应用程序打开，然后通过调用 SQLSetCursorName 进行命名的 API 服务器游标的名称。  
  
 [ @cursor_identity =] N '*input_cursor_variable*']  
 与打开的游标关联的游标变量的名称。 *input_cursor_variable* **(128) 为 nvarchar**。  
  
## <a name="return-code-values"></a>返回代码值  
 无  
  
## <a name="cursors-returned"></a>返回的游标  
 sp_describe_cursor 将其结果集封装在 [!INCLUDE[tsql](../../includes/tsql-md.md)] **游标** 输出参数中。 这样，[!INCLUDE[tsql](../../includes/tsql-md.md)] 批处理、存储过程和触发器即可逐行处理输出。 这也意味着无法直接从数据库 API 函数调用该过程。 **游标** 输出参数必须绑定到程序变量，但是数据库 api 不支持绑定 **游标** 参数或变量。  
  
 下表显示了使用 sp_describe_cursor 返回的游标的格式。 游标格式与使用 sp_cursor_list 返回的格式相同。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|reference_name|**sysname**|用于引用游标的名称。 如果通过 DECLARE CURSOR 语句中指定的名称引用游标，则引用名称与游标名称相同。 如果通过变量引用游标，则引用名称为变量的名称。|  
|cursor_name|**sysname**|来自 DECLARE CURSOR 语句的游标的名称。 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中，如果是通过将游标变量设置为游标来创建游标，则 cursor_name 返回该游标变量的名称。 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的早期版本中，此输出列可返回一个系统生成的名称。|  
|cursor_scope|**tinyint**|1 = LOCAL<br /><br /> 2 = GLOBAL|  
|状态|**int**|与 CURSOR_STATUS 系统函数报告的值相同的值：<br /><br /> 1 = 游标名称或游标变量引用的游标为打开状态。 如果游标是不敏感的、静态的或是键集，则至少具有一行。 如果游标是动态的，则结果集具有零行或多行。<br /><br /> 0 = 游标名称或游标变量引用的游标为打开状态，但不包含任何行。 动态游标从不返回此值。<br /><br /> -1 = 游标名称或游标变量引用的游标为关闭状态。<br /><br /> -2 = 仅适用于游标变量。 没有为该变量分配任何游标。 这可能是由于某个 OUTPUT 参数为该变量分配了游标，但存储过程在返回前关闭了游标。<br /><br /> -3 = 指定名称的游标或游标变量不存在，或没有为该游标变量分配游标。|  
|模型|**tinyint**|1 = 不敏感（或静态）<br /><br /> 2 = 键集<br /><br /> 3 = 动态<br /><br /> 4 = 快进|  
|concurrency|**tinyint**|1 = 只读<br /><br /> 2 = 滚动锁<br /><br /> 3 = 乐观|  
|scrollable|**tinyint**|0 = 只进<br /><br /> 1 = 可滚动|  
|open_status|**tinyint**|0 = 关闭的<br /><br /> 1 = 打开的|  
|cursor_rows|**decimal (10，0)**|结果集中合格行的数目。 有关详细信息，请参阅 [@@CURSOR_ROWS (Transact-SQL)](../../t-sql/functions/cursor-rows-transact-sql.md)。|  
|fetch_status|**smallint**|此游标上最后一次提取的状态。 有关详细信息，请参阅 [@@FETCH_STATUS (Transact-SQL)](../../t-sql/functions/fetch-status-transact-sql.md)。<br /><br /> 0 = 提取成功。<br /><br /> -1 = 提取失败或超出游标的界限。<br /><br /> -2 = 缺少所请求的行。<br /><br /> -9 = 尚未对游标进行提取。|  
|column_count|**smallint**|游标结果集中的列数。|  
|row_count|**decimal (10，0)**|受游标的上次操作影响的行数。 有关详细信息，请参阅 [@@ROWCOUNT (Transact-SQL)](../../t-sql/functions/rowcount-transact-sql.md)。|  
|last_operation|**tinyint**|对游标执行的最后一次操作：<br /><br /> 0 = 没有对游标执行操作。<br /><br /> 1 = OPEN<br /><br /> 2 = FETCH<br /><br /> 3 = 插入<br /><br /> 4 = UPDATE<br /><br /> 5 = DELETE<br /><br /> 6 = CLOSE<br /><br /> 7 = DEALLOCATE|  
|cursor_handle|**int**|服务器范围内的游标唯一值。|  
  
## <a name="remarks"></a>备注  
 sp_describe_cursor 说明服务器游标的全局特性，如滚动和更新的能力。 使用 sp_describe_cursor_columns 对游标返回的结果集的属性进行说明。 使用 sp_describe_cursor_tables 报告游标引用的基表。 若要获取连接中可见的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 服务器游标的报表，请使用 sp_cursor_list。  
  
 DECLARE CURSOR 语句可以请求一个游标类型，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 无法使用 DECLARE CURSOR 中包含的 SELECT 语句支持该游标类型。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 可以将该游标隐式转换为它可用 SELECT 语句支持的类型。 如果在 DECLARE CURSOR 语句中指定了 TYPE_WARNING，则 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将向应用程序发送一条信息性消息，说明转换已完成。 然后，可以调用 sp_describe_cursor 来确定已实现的游标类型。  
  
## <a name="permissions"></a>权限  
 要求具有 public 角色的成员身份。  
  
## <a name="examples"></a>示例  
 下面的示例将打开一个全局游标，并使用 `sp_describe_cursor` 报告该游标的属性。  
  
```  
USE AdventureWorks2012;  
GO  
-- Declare and open a global cursor.  
DECLARE abc CURSOR STATIC FOR  
SELECT LastName  
FROM Person.Person;  
  
OPEN abc;  
  
-- Declare a cursor variable to hold the cursor output variable  
-- from sp_describe_cursor.  
DECLARE @Report CURSOR;  
  
-- Execute sp_describe_cursor into the cursor variable.  
EXEC master.dbo.sp_describe_cursor @cursor_return = @Report OUTPUT,  
        @cursor_source = N'global', @cursor_identity = N'abc';  
  
-- Fetch all the rows from the sp_describe_cursor output cursor.  
FETCH NEXT from @Report;  
WHILE (@@FETCH_STATUS <> -1)  
BEGIN  
    FETCH NEXT from @Report;  
END  
  
-- Close and deallocate the cursor from sp_describe_cursor.  
CLOSE @Report;  
DEALLOCATE @Report;  
GO  
  
-- Close and deallocate the original cursor.  
CLOSE abc;  
DEALLOCATE abc;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [游标](../../relational-databases/cursors.md)   
 [CURSOR_STATUS &#40;Transact-sql&#41;](../../t-sql/functions/cursor-status-transact-sql.md)   
 [DECLARE CURSOR (Transact-SQL)](../../t-sql/language-elements/declare-cursor-transact-sql.md)   
 [sp_cursor_list &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-cursor-list-transact-sql.md)   
 [sp_describe_cursor_columns &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-describe-cursor-columns-transact-sql.md)   
 [sp_describe_cursor_tables &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-describe-cursor-tables-transact-sql.md)  
  
  
