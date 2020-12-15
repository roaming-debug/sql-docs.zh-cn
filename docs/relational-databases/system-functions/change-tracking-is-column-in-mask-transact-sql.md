---
description: CHANGE_TRACKING_IS_COLUMN_IN_MASK (Transact-SQL)
title: CHANGE_TRACKING_IS_COLUMN_IN_MASK (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 08/08/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- CHANGE_TRACKING_IS_COLUMN_IN_MASK_TSQL
- CHANGE_TRACKING_IS_COLUMN_IN_MASK
dev_langs:
- TSQL
helpviewer_keywords:
- change tracking [SQL Server], CHANGE_TRACKING_IS_COLUMN_IN_MASK
- CHANGE_TRACKING_IS_COLUMN_IN_MASK
ms.assetid: 649b370b-da54-4915-919d-1b597a39d505
author: rothja
ms.author: jroth
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: e1961dec1f32006d7a4b3d91b7b8763f34ce3514
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97440607"
---
# <a name="change_tracking_is_column_in_mask-transact-sql"></a>CHANGE_TRACKING_IS_COLUMN_IN_MASK (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  解释 CHANGETABLE (CHANGES ... ) 函数返回的 SYS_CHANGE_COLUMNS 值。 这使应用程序能够确定指定的列是否包含在为 SYS_CHANGE_COLUMNS 返回的值中。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
CHANGE_TRACKING_IS_COLUMN_IN_MASK ( column_id , change_columns )  
```  
  
## <a name="arguments"></a>自变量  
 column_id  
 是正在被检查的列的 ID。 可以使用 [COLUMNPROPERTY](../../t-sql/functions/columnproperty-transact-sql.md) 函数获取列 ID。  
  
 *change_columns*  
 是 [CHANGETABLE](../../relational-databases/system-functions/changetable-transact-sql.md) 数据的 SYS_CHANGE_COLUMNS 列中的二进制数据。  
  
## <a name="return-type"></a>返回类型  
 **bit**  
  
## <a name="return-values"></a>返回值  
 CHANGE_TRACKING_IS_COLUMN_IN_MASK 返回下列值。  
  
|返回值|说明|  
|------------------|-----------------|  
|0|指定的列不在 *change_columns* 列表中。|  
|1|指定的列位于 *change_columns* 列表。|  
  
## <a name="remarks"></a>备注  
 CHANGE_TRACKING_IS_COLUMN_IN_MASK 不会执行任何检查来验证 *column_id* 值，或者 *change_columns* 参数是从获取 *column_id* 的表中获取的。  
  
## <a name="examples"></a>示例  
 下面的示例确定是否已更新 `Salary` 表的 `Employees` 列。 `COLUMNPROPERTY`函数将返回列的列 ID `Salary` 。 必须使用 CHANGETABLE 作为数据源将 `@change_columns` 局部变量设置为查询的结果。  
  
```sql  
SET @SalaryChanged = CHANGE_TRACKING_IS_COLUMN_IN_MASK  
    (COLUMNPROPERTY(OBJECT_ID('Employees'), 'Salary', 'ColumnId')  
    ,@change_columns);  
```  
  
## <a name="see-also"></a>另请参阅  
 [变更跟踪函数 (Transact-SQL)](../../relational-databases/system-functions/change-tracking-functions-transact-sql.md)   
 [CHANGETABLE (Transact-SQL)](../../relational-databases/system-functions/changetable-transact-sql.md)   
 [跟踪数据更改 (SQL Server)](../../relational-databases/track-changes/track-data-changes-sql-server.md)  
  
  
