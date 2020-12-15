---
description: sys.sensitivity_classifications (Transact-SQL)
title: sys.sensitivity_classifications (Transact-sql) |Microsoft Docs
ms.date: 03/25/2019
ms.reviewer: ''
ms.prod: sql
ms.technology: t-sql
ms.topic: language-reference
ms.custom: ''
ms.author: mibar
author: barmichal
f1_keywords:
- 'sys.sensitivity_classifications '
dev_langs:
- TSQL
helpviewer_keywords:
- sys.sensitivity_classifications statement
- dropping labels
- drop labels
- removing labels
- remove labels
- classification [SQL]
- labels [SQL]
- information types
- rank
monikerRange: '>= sql-server-ver15 || = azuresqldb-current || = azure-sqldw-latest'
ms.openlocfilehash: 908151d19df364171063bffcd280786a354c2654
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97472948"
---
# <a name="syssensitivity_classifications-transact-sql"></a>sys.sensitivity_classifications (Transact-SQL)
[!INCLUDE [SQL Server ASDB, ASDBMI, ASDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

为数据库中的每个已分类项返回一行。

|列名称|数据类型|说明|
|-----------------|---------------|-----------------|  
|**class**|**int**|标识存在分类的项的类。 将始终具有值 1 (表示列) |  
|**class_desc**|**varchar (16)**|存在分类的项的类的说明。 始终具有值 *OBJECT_OR_COLUMN*|  
|**major_id**|**int**|表示包含已分类列的表的 ID，与 sys. _objects. object_id 相对应。|  
|**minor_id**|**int**|表示存在分类的列的 ID，与 sys. _columns. column_id 相对应。|   
|**label**|**sysname**|为敏感度分类分配的用户可读)  (标签|  
|**label_id**|**sysname**|与标签关联的 ID，可由信息保护系统（如 Azure 信息保护 (AIP）使用) |  
|**information_type**|**sysname**|为敏感度分类分配了用户可读)  (信息类型|  
|**information_type_id**|**sysname**|与信息保护系统（如 Azure 信息保护） (AIP) 相关联的信息类型的 ID|  
|**级别**|**int**|排名的数值： <br><br>0表示无<br>10表示低<br>20个用于中型<br>高30<br>40对于严重| 
|**rank_desc**|**sysname**|排名的文本表示形式：  <br><br>无、低、中、高、严重|  
| &nbsp; | &nbsp; | &nbsp; |

## <a name="remarks"></a>备注  

- 此视图提供数据库的分类状态的可见性。 它可用于管理数据库分类以及生成报告。
- 目前仅支持对数据库列进行分类。
 
## <a name="examples"></a>示例

### <a name="a-listing-all-classified-columns-and-their-corresponding-classification"></a>A. 列出所有已分类的列及其相应的分类

下面的示例返回一个表，该表列出了数据库中每个已分类列的表名称、列名称、标签、标签 ID、信息类型、信息类型 ID、排名和排名说明。

> [!NOTE]
> 标签是 Azure Synapse Analytics 的关键字。

```sql
SELECT
    SCHEMA_NAME(sys.all_objects.schema_id) as SchemaName,
    sys.all_objects.name AS [TableName], sys.all_columns.name As [ColumnName],
    [Label], [Label_ID], [Information_Type], [Information_Type_ID], [Rank], [Rank_Desc]
FROM
          sys.sensitivity_classifications
left join sys.all_objects on sys.sensitivity_classifications.major_id = sys.all_objects.object_id
left join sys.all_columns on sys.sensitivity_classifications.major_id = sys.all_columns.object_id
                         and sys.sensitivity_classifications.minor_id = sys.all_columns.column_id
```

## <a name="permissions"></a>权限  
 需要 " **查看任何敏感度分类** " 权限。 
 
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] 有关详细信息，请参阅 [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md)。  

## <a name="see-also"></a>另请参阅  

[ADD SENSITIVITY CLASSIFICATION (Transact-SQL)](../../t-sql/statements/add-sensitivity-classification-transact-sql.md)

[DROP SENSITIVITY CLASSIFICATION (Transact-SQL)](../../t-sql/statements/drop-sensitivity-classification-transact-sql.md)

[SQL 信息保护入门](/azure/azure-sql/database/data-discovery-and-classification-overview)