---
description: 'sys.masked_columns (Transact-sql) '
title: sys.masked_columns (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 02/25/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.masked_columns
- masked_columns_tsql
- sys.masked_columns_tsql
- masked_columns
helpviewer_keywords:
- sys.masked_columns catalog view
ms.assetid: 671577e4-d757-4b8d-9aa9-0fc8d51ea9ca
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: e5096f57c8e0b06c1f0403c446eb5417cd13b6f1
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095534"
---
# <a name="sysmasked_columns-transact-sql"></a>sys.masked_columns (Transact-sql) 

[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

  使用 " **sys.masked_columns** " 视图可查询应用了动态数据掩码函数的表列。 该视图继承自 **sys.columns** 视图。 该视图会返回 **sys.columns** 视图中的所有列，以及 **is_masked** 和 **masking_function** 列，表明该列是否被屏蔽，以及在该列被屏蔽的情况下定义了什么屏蔽函数。 该视图仅显示在其上应用了屏蔽函数的列。  
  
|列名称|数据类型|描述|  
|-----------------|---------------|-----------------|  
|object_id|**int**|此列所属对象的 ID。|  
|name|**sysname**|列的名称。 在对象中是唯一的。|  
|column_id|**int**|列的 ID。 在对象中是唯一的。<br /><br /> 列 ID 可以不按顺序排列。|  
|**sys.masked_columns** 返回从 **sys.databases** 继承的更多列。|各种|请参阅 [sys.databases &#40;transact-sql&#41;](../../relational-databases/system-catalog-views/sys-columns-transact-sql.md) 了解更多的列定义。|  
|is_masked|**bit**|指示是否屏蔽该列。 1表示掩码。|  
|masking_function|**nvarchar(4000)**|列的掩码函数。|  
  
## <a name="remarks"></a>备注  
  
## <a name="permissions"></a>权限  
 此视图返回有关用户对表具有某种权限或用户具有 VIEW ANY DEFINITION 权限的表的信息。  
  
## <a name="example"></a>示例  
 下面的查询将 **sys.masked_columns** 联接到 **sys.databases** 以返回有关所有屏蔽列的信息。  
  
```  
SELECT tbl.name as table_name, c.name AS column_name, c.is_masked, c.masking_function  
FROM sys.masked_columns AS c  
JOIN sys.tables AS tbl   
    ON c.object_id = tbl.object_id  
WHERE is_masked = 1;  
```  
  
## <a name="see-also"></a>另请参阅  
 [动态数据掩码](../../relational-databases/security/dynamic-data-masking.md)   
 [sys.columns (Transact-SQL)](../../relational-databases/system-catalog-views/sys-columns-transact-sql.md)  
  
  
