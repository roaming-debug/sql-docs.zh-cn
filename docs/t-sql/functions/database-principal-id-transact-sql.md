---
description: DATABASE_PRINCIPAL_ID (Transact-SQL)
title: DATABASE_PRINCIPAL_ID (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/14/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DATABASE_PRINCIPAL_ID_TSQL
- DATABASE_PRINCIPAL_ID
dev_langs:
- TSQL
helpviewer_keywords:
- identification numbers [SQL Server], principals
- principal ID numbers [SQL Server]
- DATABASE_PRINCIPAL_ID function
- IDs [SQL Server], principals
ms.assetid: 908c7dd8-c10b-4658-92f6-0224f9835dd9
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 911c4733853d3920486c2b1cd37298159b539e2d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184029"
---
# <a name="database_principal_id-transact-sql"></a>DATABASE_PRINCIPAL_ID (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

此函数返回当前数据库中的主体的 ID 号。 有关主体的详细信息，请参阅[主体（数据库引擎）](../../relational-databases/security/authentication-access/principals-database-engine.md)。
  
![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>语法  
  
```syntaxsql
DATABASE_PRINCIPAL_ID ( 'principal_name' )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
principal_name  
sysname 类型的表达式，表示数据库主体。 如果省略 principal_name，`DATABASE_PRINCIPAL_ID` 返回当前用户的 ID。 `DATABASE_PRINCIPAL_ID` 需要使用括号。
  
## <a name="return-types"></a>返回类型
**int**  
数据库主体不存在时为 NULL。
  
## <a name="remarks"></a>注解  
在所选列表、WHERE 子句或允许使用表达式的任何位置使用 `DATABASE_PRINCIPAL_ID`。 有关详细信息，请参阅[表达式 (Transact-SQL)](../../t-sql/language-elements/expressions-transact-sql.md)。
  
## <a name="examples"></a>示例  
  
### <a name="a-retrieving-the-id-of-the-current-user"></a>A. 检索当前用户的 ID  
此示例返回当前用户的数据库主体 ID。
  
```sql
SELECT DATABASE_PRINCIPAL_ID();  
GO  
```  
  
### <a name="b-retrieving-the-id-of-a-specified-database-principal"></a>B. 检索指定数据库主体的 ID  
此示例返回数据库角色 `db_owner` 的数据库主体 ID。
  
```sql
SELECT DATABASE_PRINCIPAL_ID('db_owner');  
GO  
```  
  
## <a name="see-also"></a>另请参阅
[主体（数据库引擎）](../../relational-databases/security/authentication-access/principals-database-engine.md)  
[权限层次结构（数据库引擎）](../../relational-databases/security/permissions-hierarchy-database-engine.md)  
[sys.database_principals (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md)
  
  
