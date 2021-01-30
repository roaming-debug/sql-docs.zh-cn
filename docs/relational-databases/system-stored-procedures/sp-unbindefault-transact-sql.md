---
description: sp_unbindefault (Transact-SQL)
title: sp_unbindefault (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_unbindefault_TSQL
- sp_unbindefault
dev_langs:
- TSQL
helpviewer_keywords:
- sp_unbindefault
ms.assetid: c96a6c5e-f3ca-4c1e-b64b-0d8ef6986af8
author: markingmyname
ms.author: maghan
ms.openlocfilehash: ecc8a6c7a012a36da4deeacf8b4e66e6096c8227
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202347"
---
# <a name="sp_unbindefault-transact-sql"></a>sp_unbindefault (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  在当前数据库中为列或者别名数据类型解除（删除）默认值绑定。  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepNextDontUse](../../includes/ssnotedepnextdontuse-md.md)] 建议您改为使用 [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md) 或 [CREATE TABLE](../../t-sql/statements/create-table-transact-sql.md) 语句中的 default 关键字来创建默认定义。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_unbindefault [ @objname = ] 'object_name'   
     [ , [ @futureonly = ] 'futureonly_flag' ]  
```  
  
## <a name="arguments"></a>参数  
`[ @objname = ] 'object_name'` 要取消其默认值绑定的表和列或别名数据类型的名称。 *object_name* 为 **nvarchar (776)**，无默认值。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 尝试先将两部分标识符解析为列名，再解析为别名数据类型。  
  
 解除别名数据类型的默认值绑定时，也同时解除数据类型相同且具有相同默认值的任何列的默认值绑定。 属于该数据类型并且直接绑定默认值的类将不受影响。  
  
> [!NOTE]  
>  *object_name* 可以将方括号 **[]** 包含为分隔的标识符字符。 有关详细信息，请参阅 [Database Identifiers](../../relational-databases/databases/database-identifiers.md)。  
  
`[ @futureonly = ] 'futureonly_flag'` 仅在解除别名数据类型的默认值绑定时使用。 *futureonly_flag* 是 **varchar (15)**，默认值为 NULL。 *Futureonly_flag* **futureonly** 时，数据类型的现有列不会丢失指定的默认值。  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或 1（失败）  
  
## <a name="remarks"></a>备注  
 若要显示默认值的文本，请执行 **sp_helptext** ，并使用默认的作为参数。  
  
## <a name="permissions"></a>权限  
 若要解除表列的默认值绑定，需要对表具有 ALTER 权限。 若要解除别名数据类型的默认值绑定，需要对该类型具有 CONTROL 权限或对该类型所属架构具有 ALTER 权限。  
  
## <a name="examples"></a>示例  
  
### <a name="a-unbinding-a-default-from-a-column"></a>A. 解除列的默认值绑定  
 以下示例解除 `hiredate` 表的 `employees` 列的默认值绑定。  
  
```  
EXEC sp_unbindefault 'employees.hiredate';  
```  
  
### <a name="b-unbinding-a-default-from-an-alias-data-type"></a>B. 解除别名数据类型的默认值绑定  
 以下示例解除别名数据类型 `ssn` 的默认值绑定。 这将为该数据类型的现有列和将来的列解除绑定。  
  
```  
EXEC sp_unbindefault 'ssn';  
```  
  
### <a name="c-using-the-futureonly_flag"></a>C. 使用 futureonly_flag  
 以下示例解除将来使用别名数据类型 `ssn` 时的绑定，而不影响现有 `ssn` 列。  
  
```  
EXEC sp_unbindefault 'ssn', 'futureonly';  
```  
  
### <a name="d-using-delimited-identifiers"></a>D. 使用分隔标识符  
 下面的示例演示如何在 *object_name* 参数中使用分隔标识符。  
  
```  
CREATE TABLE [t.3] (c1 int); -- Notice the period as part of the table   
-- name.  
CREATE DEFAULT default2 AS 0;  
GO  
EXEC sp_bindefault 'default2', '[t.3].c1' ;  
-- The object contains two periods;  
-- the first is part of the table name and the second   
-- distinguishes the table name from the column name.  
EXEC sp_unbindefault '[t.3].c1';  
```  
  
## <a name="see-also"></a>另请参阅  
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [数据库引擎存储过程 &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [CREATE DEFAULT (Transact-SQL)](../../t-sql/statements/create-default-transact-sql.md)   
 [DROP DEFAULT (Transact-SQL)](../../t-sql/statements/drop-default-transact-sql.md)   
 [sp_bindefault (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-bindefault-transact-sql.md)   
 [sp_helptext (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-helptext-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
