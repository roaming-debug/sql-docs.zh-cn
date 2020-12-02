---
description: DROP TYPE (Transact-SQL)
title: DROP TYPE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/12/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DROP TYPE
- DROP_TYPE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- user-defined types [SQL Server], deleting
- UDTs [SQL Server], deleting
- alias data types [SQL Server], removing
- DROP TYPE statement
ms.assetid: 11bf83f9-0718-4238-a835-83d2eb14ae7b
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 26fb837dc28b0ac7b2845a64eab800512e508b7b
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96131067"
---
# <a name="drop-type-transact-sql"></a>DROP TYPE (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  从当前数据库中删除别名数据类型或公共语言运行时 (CLR) 用户定义的类型。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
DROP TYPE [ IF EXISTS ] [ schema_name. ] type_name [ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 IF EXISTS  
 适用范围：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 到[当前版本](https://go.microsoft.com/fwlink/p/?LinkId=299658)）。  
  
 只有在类型已存在时才对其进行有条件地删除。  
  
 *schema_name*  
 别名或用户定义的类型所属的架构名。  
  
 type_name  
 要删除的别名数据类型或用户定义的类型的名称。  
  
## <a name="remarks"></a>备注  
 在满足以下任何条件的情况下，将不执行 DROP TYPE 语句：  
  
-   数据库中存在包含别名数据类型列或用户定义的类型列的表。 通过查询 [sys.columns](../../relational-databases/system-catalog-views/sys-columns-transact-sql.md) 或 [sys.column_type_usages](../../relational-databases/system-catalog-views/sys-column-type-usages-transact-sql.md) 目录视图可获得有关别名类型列或用户定义类型列的信息。  
  
-   存在定义中引用了别名类型和用户定义类型的计算列、CHECK 约束、架构绑定视图和绑定到架构的函数。 通过查询 [sys.sql_expression_dependencies](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md) 目录视图可获得有关这些引用的信息。  
  
-   存在在数据库中创建的函数、存储过程或触发器，且这些例程使用别名类型或用户定义的类型的变量和参数。 通过查询 [sys.parameters](../../relational-databases/system-catalog-views/sys-parameters-transact-sql.md) 或 [sys.parameter_type_usages](../../relational-databases/system-catalog-views/sys-parameter-type-usages-transact-sql.md) 目录视图可获得有关别名类型参数或用户定义类型参数的信息。  
  
## <a name="permissions"></a>权限  
 需要对 type_name 拥有 CONTROL 权限，或对 schema_name 拥有 ALTER 权限。  
  
## <a name="examples"></a>示例  
 以下示例假设已经在当前数据库中创建了一个名为 `ssn` 的类型。  
  
```sql  
DROP TYPE ssn ;  
```  
  
## <a name="see-also"></a>另请参阅  
 [CREATE TYPE (Transact-SQL)](../../t-sql/statements/create-type-transact-sql.md)   
 [EVENTDATA (Transact-SQL)](../../t-sql/functions/eventdata-transact-sql.md)  
  
  
