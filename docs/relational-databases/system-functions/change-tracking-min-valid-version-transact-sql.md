---
description: CHANGE_TRACKING_MIN_VALID_VERSION (Transact-SQL)
title: CHANGE_TRACKING_MIN_VALID_VERSION (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 08/08/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- CHANGE_TRACKING_CLEANUP_VERSION
- CHANGE_TRACKING_CLEANUP_VERSION_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- CHANGE_TRACKING_MIN_VALID_VERSION
- change tracking [SQL Server], CHANGE_TRACKING_MIN_VALID_VERSION
ms.assetid: 5a43d23f-adcf-4c0b-95ad-07cee03c1f9d
author: rothja
ms.author: jroth
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: d1a3144ed51592d3014564d7d5b9690fed958fb9
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97440598"
---
# <a name="change_tracking_min_valid_version-transact-sql"></a>CHANGE_TRACKING_MIN_VALID_VERSION (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  当使用 [CHANGETABLE](../../relational-databases/system-functions/changetable-transact-sql.md) 函数时，返回客户端上可用于从指定表获取更改跟踪信息的最小版本。  
    
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
CHANGE_TRACKING_MIN_VALID_VERSION ( table_object_id )  
```  
  
## <a name="arguments"></a>自变量  
 *table_object_id*  
 表的对象 ID。 *table_object_id* 是 **int**。  
  
## <a name="return-type"></a>返回类型  
 **bigint**  
  
## <a name="remarks"></a>备注  
 使用此函数验证 CHANGETABLE 的 *last_sync_version* 参数的值。 如果 *last_sync_version* 小于此函数报告的值，则稍后调用 CHANGETABLE 后返回的结果可能无效。  
  
 CHANGE_TRACKING_MIN_VALID_VERSION 使用以下信息来确定返回值：  
  
-   为此表启用更改跟踪的时间。  
  
-   何时运行后台清除任务来删除超过了数据库的指定保持期的更改跟踪信息。  
  
-   此表是否已被截断。 这会删除与此表关联的所有更改跟踪信息。  
  
 如果符合下列任一条件，该函数将返回 NULL：  
  
-   没有为数据库启用更改跟踪。  
  
-   指定表的对象 ID 对当前数据库无效。  
  
-   对由此对象 ID 指定的表拥有的权限不足。  
  
## <a name="examples"></a>示例  
 下面的示例确定指定的版本是否为有效版本。 此示例获取 `dbo.Employees` 表的最低有效版本，然后将其与 `@last_sync_version` 变量的值进行比较。 如果 `@last_sync_version` 的值低于 `@min_valid_version` 的值，则已更改行的列表将无效。  
  
> [!NOTE]  
>  通常，您可以从存储用来同步数据的上一个版本号的表或其他位置获取此值。  
  
```  
-- The tracked change is tagged with the specified context   
DECLARE @min_valid_version bigint, @last_sync_version bigint;  
  
SET @min_valid_version =   
CHANGE_TRACKING_MIN_VALID_VERSION(OBJECT_ID('dbo.Employees'));  
  
SET @last_sync_version = 11  
IF (@last_sync_version < @min_valid_version)  
-- Error � do not obtain changes  
ELSE  
-- Obtain changes using CHANGETABLE(CHANGES ...)  
```  
  
## <a name="see-also"></a>另请参阅  
 [变更跟踪函数 (Transact-SQL)](../../relational-databases/system-functions/change-tracking-functions-transact-sql.md)   
 [sys.change_tracking_tables (Transact-SQL)](../../relational-databases/system-catalog-views/change-tracking-catalog-views-sys-change-tracking-tables.md)  
  
  
