---
description: sp_bindefault (Transact-SQL)
title: sp_bindefault (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 11/25/2015
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sp_bindefault
- sp_bindefault_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_bindefault
ms.assetid: 3da70c10-68d0-4c16-94a5-9e84c4a520f6
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 9cad51a7a83f694ac89b41584929a46e1fbc725c
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97482412"
---
# <a name="sp_bindefault-transact-sql"></a>sp_bindefault (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  将默认值绑定到列或绑定到别名数据类型。  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureDontUse](../../includes/ssnotedepfuturedontuse-md.md)] 建议改为使用 [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md) 或 [CREATE TABLE](../../t-sql/statements/create-table-transact-sql.md) 语句的 default 关键字来创建默认定义。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_bindefault [ @defname = ] 'default' ,   
    [ @objname = ] 'object_name'   
    [ , [ @futureonly = ] 'futureonly_flag' ]   
```  
  
## <a name="arguments"></a>自变量  
`[ @defname = ] 'default'` 由 CREATE DEFAULT 创建的默认值的名称。 *默认值* 为 **nvarchar (776)**，无默认值。  
  
`[ @objname = ] 'object_name'` 要绑定默认值的表和列的名称或别名数据类型。 *object_name* 为 **nvarchar (776)** ，无默认值。 不能用 **varchar (max)**、 **nvarchar (max)**、 **varbinary (MAX**) 、 **xml** 或 CLR 用户定义类型来定义 *object_name* 。  
  
 如果 *object_name* 是由一个部分组成的名称，则将其解析为别名数据类型。 如果它是由两部分或三部分组成的名称，则首先将它解析为表和列;如果此分辨率失败，则将其解析为别名数据类型。 默认情况下，别名数据类型的现有列将继承 *默认值*，除非已将默认值直接绑定到列。 不能将默认值绑定到 **text**、 **ntext**、 **image**、 **varchar (max)**、 **nvarchar (max)**、 **varbinary (max)**、 **xml**、 **timestamp** 或 CLR 用户定义类型列、具有 IDENTITY 属性的列、计算列或已具有 default 约束的列。  
  
> [!NOTE]  
>  *object_name* 可以包含方括号 **[]** 作为分隔标识符。 有关详细信息，请参阅 [Database Identifiers](../../relational-databases/databases/database-identifiers.md)。  
  
`[ @futureonly = ] 'futureonly_flag'` 仅在将默认值绑定到别名数据类型时使用。 *futureonly_flag* 是 **varchar (15)** ，默认值为 NULL。 当此参数设置为 **futureonly** 时，该数据类型的现有列无法继承新的默认值。 将默认值绑定到列时，从不使用此参数。 如果 *futureonly_flag* 为 NULL，则新的默认值将绑定到当前没有默认值或使用别名数据类型的现有默认值的别名数据类型的任何列。  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或 1（失败）  
  
## <a name="remarks"></a>备注  
 您可以使用 **sp_bindefault** 将新的默认值绑定到列，但最好使用默认约束，或将其绑定到别名数据类型，而不解除现有默认值的绑定。 原有默认值将被覆盖。 不能将默认值绑定到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 系统数据类型或 CLR 用户定义类型。 如果默认值和要绑定到的列不兼容，那么在试图插入默认值时（不是在绑定时），[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 将返回错误消息。  
  
 别名数据类型的现有列将继承新的默认值，除非默认值直接绑定到这些列，否则将 *futureonly_flag* 指定为 **futureonly**。 别名数据类型的新列始终继承默认值。  
  
 将默认值绑定到列时，相关信息将添加到 **sys.databases** 目录视图中。 将默认值绑定到别名数据类型时，相关信息将添加到 **sys.databases** 目录视图中。  
  
## <a name="permissions"></a>权限  
 用户必须是表的成员，或者是 **sysadmin** 固定服务器角色的成员，或者是 **db_owner** 和 **db_ddladmin** 固定数据库角色的成员。  
  
## <a name="examples"></a>示例  
  
### <a name="a-binding-a-default-to-a-column"></a>A. 将默认值绑定到列  
 已经使用 CREATE DEFAULT 在当前数据库中定义了名为 `today` 的默认值，以下示例将此默认值绑定到 `HireDate` 表的 `Employee` 列。 每当将行添加到 `Employee` 表而且没有提供 `HireDate` 列的数据时，列都取得默认值 `today` 的值。  
  
```  
USE master;  
GO  
EXEC sp_bindefault 'today', 'HumanResources.Employee.HireDate';  
```  
  
### <a name="b-binding-a-default-to-an-alias-data-type"></a>B. 将默认值绑定到别名数据类型  
 已经存在名为 `def_ssn` 的默认值和名为 `ssn` 的别名数据类型。 以下示例将默认值 `def_ssn` 绑定到 `ssn`。 创建表时，被赋予别名数据类型 `ssn` 的所有列都将继承此默认值。 类型为 **ssn** 的现有列还继承默认 **def_ssn**，除非为 *futureonly_flag* 值指定 **futureonly** ，否则，除非该列有直接绑定到它的默认值。 绑定到列的默认值始终优先于绑定到数据类型的默认值。  
  
```  
USE master;  
GO  
EXEC sp_bindefault 'def_ssn', 'ssn';  
```  
  
### <a name="c-using-the-futureonly_flag"></a>C. 使用 futureonly_flag  
 以下示例将默认值 `def_ssn` 绑定到别名数据类型 `ssn`。 由于指定了 **futureonly** ，因此不会影响类型为的现有列 `ssn` 。  
  
```  
USE master;  
GO  
EXEC sp_bindefault 'def_ssn', 'ssn', 'futureonly';  
```  
  
### <a name="d-using-delimited-identifiers"></a>D. 使用分隔标识符  
 下面的示例演示如何 `[t.1]` 在 *object_name* 中使用分隔标识符。  
  
```  
USE master;  
GO  
CREATE TABLE [t.1] (c1 int);   
-- Notice the period as part of the table name.  
EXEC sp_bindefault 'default1', '[t.1].c1' ;  
-- The object contains two periods;   
-- the first is part of the table name,   
-- and the second distinguishes the table name from the column name.  
```  
  
## <a name="see-also"></a>另请参阅  
 [数据库引擎存储过程 &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [CREATE DEFAULT (Transact-SQL)](../../t-sql/statements/create-default-transact-sql.md)   
 [DROP DEFAULT (Transact-SQL)](../../t-sql/statements/drop-default-transact-sql.md)   
 [sp_unbindefault &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-unbindefault-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
