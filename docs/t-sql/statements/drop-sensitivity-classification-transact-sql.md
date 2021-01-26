---
description: DROP SENSITIVITY CLASSIFICATION (Transact-SQL)
title: DROP SENSITIVITY CLASSIFICATION (Transact-SQL) | Microsoft Docs
ms.date: 03/25/2019
ms.reviewer: ''
ms.prod: sql
ms.technology: t-sql
ms.topic: language-reference
ms.custom: ''
ms.author: giladm
author: giladmit
f1_keywords:
- DROP SENSITIVITY CLASSIFICATION
- DROP_SENSITIVITY_CLASSIFICATION
dev_langs:
- TSQL
helpviewer_keywords:
- DROP SENSITIVITY CLASSIFICATION statement
- dropping labels
- drop labels
- removing labels
- remove labels
- classification [SQL]
- labels [SQL]
- information types
- data classification
monikerRange: " >= sql-server-ver15 || = azuresqldb-current"
ms.openlocfilehash: 8b5dce1e51adfd52c89a7cc6fbba2d9f91497933
ms.sourcegitcommit: 713e5a709e45711e18dae1e5ffc190c7918d52e7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/22/2021
ms.locfileid: "98688910"
---
# <a name="drop-sensitivity-classification-transact-sql"></a>DROP SENSITIVITY CLASSIFICATION (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

从一个或多个数据库列中删除敏感度分类元数据。

## <a name="syntax"></a>语法

```syntaxsql
DROP SENSITIVITY CLASSIFICATION FROM
    <object_name> [, ...n ]

<object_name> ::=
{
    [schema_name.]table_name.column_name
}
```  

## <a name="arguments"></a>参数  

object_name ([schema_name.]table_name.column_name)

是从中删除分类的数据库列的名称。 目前仅支持列分类。
    - schema_name（可选）- 是已分类的列所属架构的名称。
    - *table_name* - 是已分类的列所属表的名称。
    - column_name - 是从中删除分类的列的名称。

## <a name="remarks"></a>备注  

- 可以使用单个“DROP SENSITIVITY CLASSIFICATION”语句删除多个对象分类。

## <a name="permissions"></a>权限  

需要“ALTER ANY SENSITIVITY CLASSIFICATION”权限。 “ALTER ANY SENSITIVITY CLASSIFICATION”由数据库权限“ALTER”或服务器权限“CONTROL SERVER”表示。


## <a name="examples"></a>示例  


### <a name="a-dropping-classification-from-a-single-column"></a>A. 从单个列中删除分类

以下示例从列 `dbo.sales.price` 中删除分类。  

```sql
DROP SENSITIVITY CLASSIFICATION FROM
    dbo.sales.price
```

### <a name="b-dropping-classification-from-multiple-columns"></a>B. 从多个列中删除分类

以下示例从列 `dbo.sales.price``dbo.sales.discount` 和 `SalesLT.Customer.Phone` 中删除分类。  

```sql
DROP SENSITIVITY CLASSIFICATION FROM
    dbo.sales.price, dbo.sales.discount, SalesLT.Customer.Phone  
```

## <a name="see-also"></a>另请参阅  

[ADD SENSITIVITY CLASSIFICATION (Transact-SQL)](../../t-sql/statements/add-sensitivity-classification-transact-sql.md)

[sys.sensitivity_classifications (Transact-SQL)](../../relational-databases/system-catalog-views/sys-sensitivity-classifications-transact-sql.md)

[SQL 信息保护入门](/azure/azure-sql/database/data-discovery-and-classification-overview)