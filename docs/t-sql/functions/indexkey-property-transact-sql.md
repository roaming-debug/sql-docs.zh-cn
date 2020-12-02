---
description: INDEXKEY_PROPERTY (Transact-SQL)
title: INDEXKEY_PROPERTY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- INDEXKEY_PROPERTY_TSQL
- INDEXKEY_PROPERTY
dev_langs:
- TSQL
helpviewer_keywords:
- index keys [SQL Server]
- INDEXKEY_PROPERTY function
- viewing index keys
- displaying index keys
- keys [SQL Server], index
ms.assetid: 87c0c385-6b2d-4716-ac8c-a3ce6e8d89e9
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 60247a9c2dcdc038f17edc9637805c62e02fe0d0
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "91115438"
---
# <a name="indexkey_property-transact-sql"></a>INDEXKEY_PROPERTY (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  返回有关索引键的信息。 对于 XML 索引，返回 NULL。  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] 相反，则使用 [sys.index_columns (Transact-SQL)](../../relational-databases/system-catalog-views/sys-index-columns-transact-sql.md)。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
INDEXKEY_PROPERTY ( object_ID ,index_ID ,key_ID ,property )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 object_ID  
 表或索引视图的对象标识号。 object_id 的数据类型为 int。  
  
 index_ID  
 索引标识号。 index_ID 的数据类型为 int。  
  
 key_ID  
 索引键列的位置。 key_ID 的数据类型为 int。  
  
 *property*  
 要返回其信息的属性的名称。 property 是字符串，可以是下列值之一。  
  
|值|描述|  
|-----------|-----------------|  
|**ColumnId**|索引的 key_ID 位置上的列 ID。|  
|**IsDescending**|存储索引列的排序顺序。<br /><br /> 1 = 降序 0 = 升序|  
  
## <a name="return-types"></a>返回类型  
 **int**  
  
## <a name="exceptions"></a>例外  
 出现错误时或调用方没有查看对象的权限时，将返回 NULL。  
  
 用户只能查看符合如下条件的安全对象的元数据：该安全对象为该用户所有，或已授予该用户对该安全对象的权限。 也就是说，如果用户对该对象没有任何权限，则那些会生成元数据的内置函数（如 INDEXKEY_PROPERTY）可能返回 NULL。 有关详细信息，请参阅 [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md)。  
  
## <a name="examples"></a>示例  
 在以下示例中，将返回 `1` 表中索引 ID `1` 和键列 `Production.Location` 的两个属性。  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT   
    INDEXKEY_PROPERTY(OBJECT_ID('Production.Location', 'U'),  
        1,1,'ColumnId') AS [Column ID],  
    INDEXKEY_PROPERTY(OBJECT_ID('Production.Location', 'U'),  
        1,1,'IsDescending') AS [Asc or Desc order];  
```  
  
 下面是结果集：  
  
```  
Column ID   Asc or Desc order   
----------- -----------------   
1           0  
  
(1 row(s) affected)  
```  
  
## <a name="see-also"></a>另请参阅  
 [INDEX_COL (Transact-SQL)](../../t-sql/functions/index-col-transact-sql.md)   
 [INDEXPROPERTY (Transact-SQL)](../../t-sql/functions/indexproperty-transact-sql.md)   
 [sys.objects (Transact-SQL)](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)   
 [sys.indexes (Transact-SQL)](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)   
 [sys.index_columns (Transact-SQL)](../../relational-databases/system-catalog-views/sys-index-columns-transact-sql.md)  
  
  
