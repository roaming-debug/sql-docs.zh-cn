---
description: ADD SENSITIVITY CLASSIFICATION (Transact-SQL)
title: ADD SENSITIVITY CLASSIFICATION (Transact-SQL)
ms.prod: sql
ms.technology: t-sql
ms.topic: reference
author: DavidTrigano
ms.author: datrigan
ms.reviewer: vanto
f1_keywords:
- ADD SENSITIVITY CLASSIFICATION
- ADD_SENSITIVITY_CLASSIFICATION
helpviewer_keywords:
- ADD SENSITIVITY CLASSIFICATION statement
- add labels
- adding labels
- adding labels
- classification [SQL]
- labels [SQL]
- information types
- data classification
- rank
ms.custom: ''
ms.date: 06/10/2020
monikerRange: " >= sql-server-linux-ver15 || >= sql-server-ver15 || = azuresqldb-current"
ms.openlocfilehash: 09bf980c0bfa6e07a9d2de35519439c8b18da039
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348578"
---
# <a name="add-sensitivity-classification-transact-sql"></a>ADD SENSITIVITY CLASSIFICATION (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

将有关敏感度分类的元数据添加到一个或多个数据库列中。 分类可以包括敏感度标签和信息类型。

对于 SQL Server，这在 SQL Server 2019 中进行了引入。

对你的数据库环境中的敏感数据进行分类可帮助实现更高的可见性和更好的保护。 可以在 [SQL 信息保护入门](/azure/azure-sql/database/data-discovery-and-classification-overview)中找到更多信息

## <a name="syntax"></a>语法

```syntaxsql
    ADD SENSITIVITY CLASSIFICATION TO
    <object_name> [, ...n ]
    WITH ( <sensitivity_option> [, ...n ] )

<object_name> ::=
{
    [schema_name.]table_name.column_name
}

<sensitivity_option> ::=  
{
    LABEL = string |
    LABEL_ID = guidOrString |
    INFORMATION_TYPE = string |
    INFORMATION_TYPE_ID = guidOrString |
    RANK = NONE | LOW | MEDIUM | HIGH | CRITICAL
}
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数  

object_name ([schema_name.]table_name.column_name)

是要进行分类的数据库列的名称。 目前仅支持列分类。
    - schema_name（可选）- 是已分类的列所属架构的名称。
    - *table_name* - 是已分类的列所属表的名称。
    - column_name - 是正在进行分类的列的名称。

*LABEL*

是敏感度标签的可人工读取名称。 敏感度标签表示数据库列中存储的数据的敏感度。

LABEL_ID

是与敏感度标签相关联的标识符。 这通常由集中式信息保护平台用于唯一标识系统中的标签。

INFORMATION_TYPE

是信息类型的可人工读取名称。 信息类型用于描述存储在数据库列中数据的类型。

INFORMATION_TYPE_ID

是与信息类型相关联的标识符。 这通常由集中式信息保护平台用于唯一标识系统中的信息类型。

*RANK*

是基于预定义的一组值的标识符，这组值定义敏感度等级。 由高级威胁防护等其他服务用于根据其级别来检测异常。

## <a name="remarks"></a>备注  

- 只能向单个对象添加一个分类。 向已进行分类的对象添加分类将覆盖现有分类。
- 可以使用单个 `ADD SENSITIVITY CLASSIFICATION` 语句对多个对象进行分类。
- 系统视图 [sys.sensitivity_classifications](../../relational-databases/system-catalog-views/sys-sensitivity-classifications-transact-sql.md) 可用于检索数据库的敏感度分类信息。

## <a name="permissions"></a>权限

需要“ALTER ANY SENSITIVITY CLASSIFICATION”权限。 “ALTER ANY SENSITIVITY CLASSIFICATION”由数据库权限“ALTER”或服务器权限“CONTROL SERVER”表示。

## <a name="examples"></a>示例  

### <a name="a-classifying-two-columns"></a>A. 对两个列进行分类

    以下示例使用敏感度标签“高度机密”、级别“严重”和信息类型“财务”对列 dbo.sales.price 和 dbo.sales.discount 进行分类。

```sql
ADD SENSITIVITY CLASSIFICATION TO
    dbo.sales.price, dbo.sales.discount
    WITH ( LABEL='Highly Confidential', INFORMATION_TYPE='Financial', RANK=CRITICAL )
```  

### <a name="b-classifying-only-a-label"></a>B. 仅对一个标签进行分类

以下示例使用标签“机密”和标签 ID 643f7acd-776a-438d-890c-79c3f2a520d6 对列 dbo.customer.comments 进行分类。 未对此列进行信息类型分类。

```sql
ADD SENSITIVITY CLASSIFICATION TO
    dbo.customer.comments
    WITH ( LABEL='Confidential', LABEL_ID='643f7acd-776a-438d-890c-79c3f2a520d6' )
```  

## <a name="see-also"></a>另请参阅

- [DROP SENSITIVITY CLASSIFICATION (Transact-SQL)](../../t-sql/statements/drop-sensitivity-classification-transact-sql.md)
- [sys.sensitivity_classifications (Transact-SQL)](../../relational-databases/system-catalog-views/sys-sensitivity-classifications-transact-sql.md)
- [权限（数据库引擎）](../../relational-databases/security/permissions-database-engine.md)
- [SQL 信息保护入门](/azure/azure-sql/database/data-discovery-and-classification-overview)