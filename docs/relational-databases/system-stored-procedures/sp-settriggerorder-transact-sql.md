---
description: sp_settriggerorder (Transact-SQL)
title: sp_settriggerorder (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_settriggerorder
- sp_settriggerorder_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_settriggerorder
ms.assetid: 8b75c906-7315-486c-bc59-293ef12078e8
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: f7977873f4b2887b736ffc16f5592340f5b87dee
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207368"
---
# <a name="sp_settriggerorder-transact-sql"></a>sp_settriggerorder (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  指定第一个激发或最后一个激发的 AFTER 触发器。 在第一个和最后一个触发器之间激发的 AFTER 触发器将按未定义的顺序执行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_settriggerorder [ @triggername = ] '[ triggerschema. ] triggername'   
    , [ @order = ] 'value'   
    , [ @stmttype = ] 'statement_type'   
    [ , [ @namespace = ] { 'DATABASE' | 'SERVER' | NULL } ]  
```  
  
## <a name="arguments"></a>参数  
`[ @triggername = ] '[ _triggerschema.] _triggername'` 触发器的名称及其所属的架构（如果适用），其顺序为 "已设置" 或 "已更改"。 [_triggerschema_**.**]*triggername* 为 **sysname**。 如果名称与触发器不对应，或者名称与 INSTEAD OF 触发器对应，则该过程将返回错误。 不能为 DDL 或登录触发器指定 *triggerschema* 。  
  
`[ @order = ] 'value'` 触发器的新顺序的设置。 *值* 为 **varchar (10)** ，可以是以下值之一。  
  
> [!IMPORTANT]  
>  **第一个** 和 **最后一个** 触发器必须是两个不同的触发器。  
  
|值|描述|  
|-----------|-----------------|  
|**第一个**|触发器被第一个触发。|  
|**上一次**|触发器被最后一个触发。|  
|无|触发器以未定义的顺序触发。|  
  
`[ @stmttype = ] 'statement_type'` 指定触发触发器的 SQL 语句。 *statement_type* 是 **varchar (50)** ，可以是 INSERT、UPDATE、DELETE、LOGON 或 [!INCLUDE[tsql](../../includes/tsql-md.md)] [DDL 事件](../../relational-databases/triggers/ddl-events.md)中列出的任何语句事件。 不能指定事件组。  
  
 只有在将触发器定义为该语句类型的触发器之后，才能将该触发器指定为语句类型的 **第一个** 或 **最后** 一个触发器。 例如，如果 **TR1** 定义为 insert 触发器，则可以 **先** 将 trigger **TR1** 指定为 table **T1** 上的 insert。 [!INCLUDE[ssDE](../../includes/ssde-md.md)]如果仅将 **TR1** 定义为 INSERT 触发器，则会将设置为 UPDATE 语句的 **第一个** 或 **最后** 一个触发器。 有关详细信息，请参见“备注”部分。  
  
 **\@ namespace =** { **' 数据库 '**  |  **"SERVER"** |无效  
 当 *triggername* 是 DDL 触发器时， **\@ 命名空间** 指定是使用数据库作用域还是服务器作用域创建 *triggername* 。 如果 *triggername* 是 logon 触发器，则必须指定服务器。 有关 DDL 触发器作用域的详细信息，请参阅 [Ddl 触发器](../../relational-databases/triggers/ddl-triggers.md)。 如果未指定或指定 NULL，则 *triggername* 是 DML 触发器。  
  
* 服务器适用于： [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 和更高版本。
  
## <a name="return-code-values"></a>返回代码值  
 0 (成功) 和 1 (失败)   
  
## <a name="remarks"></a>备注  
  
## <a name="dml-triggers"></a>DML 触发器  
 对于单个表中的每个语句，只能有一个和 **最后****一个触发器**。  
  
 如果已在表、数据库或服务器上定义了 **第一个** 触发器，则不能为同一个 *statement_type* 为 **同一** 表、数据库或服务器指定新的触发器。 此限制也适用于 **最后一个** 触发器。  
  
 复制将为包含在立即更新订阅或排队更新订阅中的任意表自动生成第一个触发器。 复制要求其触发器为第一个触发器。 在尝试将带有第一个触发器的表包含在立即更新订阅或排队更新订阅中时，复制将引发错误。 如果在表已经包含在订阅中之后尝试使某个触发器成为第一个触发器， **sp_settriggerorder** 将返回错误。 如果在复制触发器上使用 ALTER TRIGGER，或使用 **sp_settriggerorder** 将复制触发器更改为 **最后** 一个触发器或 **无** 触发器，则订阅将无法正常运行。  
  
## <a name="ddl-triggers"></a>DDL 触发器  
 如果具有数据库作用域的 DDL 触发器和具有服务器作用域的 DDL 触发器存在于同一事件上，则可以指定两个触发器都是 **第一个** 触发器或 **最后** 一个触发器。 但是，服务器作用域的触发器始终最先触发。 一般情况下，同一事件中 DDL 触发器的执行顺序如下：  
  
1.  标记为 **First** 的服务器级触发器。  
  
2.  其他服务器级触发器。  
  
3.  标记为 **Last** 的服务器级触发器。  
  
4.  标记为 **First** 的数据库级触发器。  
  
5.  其他数据库级触发器。  
  
6.  标记为 **Last** 的数据库级触发器。  
  
## <a name="general-trigger-considerations"></a>常规触发器注意事项  
 如果 ALTER TRIGGER 语句更改了第一个或最后一个触发器，则会删除最初在触发器上设置的 **第一个** 或 **最后** 一个属性，并且值将替换为 " **无**"。 必须使用 **sp_settriggerorder** 重置顺序值。  
  
 如果必须将同一个触发器指定为多个语句类型的第一个或最后一个顺序，则必须为每个语句类型执行 **sp_settriggerorder** 。 此外，必须首先为语句类型定义触发器，然后才能将该触发器指定为要为该语句类型激发的 **第一个** 或 **最后一个** 触发器。  
  
## <a name="permissions"></a>权限  
 若要设置具有服务器作用域（使用 ON ALL SERVER 创建）的 DDL 触发器或登录触发器的顺序，需要具有 CONTROL SERVER 权限。  
  
 若要设置具有数据库作用域（使用 ON DATABASE 创建）的 DDL 触发器的顺序，需要具有 ALTER ANY DATABASE DDL TRIGGER 权限。  
  
 若要设置 DML 触发器的顺序，需要对要在其中定义该触发器的表或视图具有 ALTER 权限。  
  
## <a name="examples"></a>示例  
  
### <a name="a-setting-the-firing-order-for-a-dml-trigger"></a>A. 设置 DML 触发器的触发顺序  
 以下示例指定触发器 `uSalesOrderHeader` 是对 `UPDATE` 表执行 `Sales.SalesOrderHeader` 操作后触发的第一个触发器。  
  
```  
USE AdventureWorks2012;  
GO  
sp_settriggerorder @triggername= 'Sales.uSalesOrderHeader', @order='First', @stmttype = 'UPDATE';  
```  
  
### <a name="b-setting-the-firing-order-for-a-ddl-trigger"></a>B. 设置 DDL 触发器的触发顺序  
 以下示例指定触发器 `ddlDatabaseTriggerLog` 是对 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库执行 `ALTER_TABLE` 操作后触发的第一个触发器。  
  
```  
USE AdventureWorks2012;  
GO  
sp_settriggerorder @triggername= 'ddlDatabaseTriggerLog', @order='First', @stmttype = 'ALTER_TABLE', @namespace = 'DATABASE';  
```  
  
## <a name="see-also"></a>另请参阅  
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [数据库引擎存储过程 &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [ALTER TRIGGER (Transact-SQL)](../../t-sql/statements/alter-trigger-transact-sql.md)  
  
  
