---
description: sys.fn_cdc_get_column_ordinal (Transact-SQL)
title: sys.fn_cdc_get_column_ordinal (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 01/25/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.fn_cdc_get_column_ordinal
- fn_cdc_get_column_ordinal_TSQL
- fn_cdc_get_column_ordinal
- sys.fn_cdc_get_column_ordinal_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- fn_cdc_get_column_ordinal
- sys.fn_cdc_get_column_ordinal
ms.assetid: 4bb21a57-2b94-4208-8bdf-6a3e2681d881
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 9b778fca1203a7330b1d9f403a31e38ce71433c0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194453"
---
# <a name="sysfn_cdc_get_column_ordinal-transact-sql"></a>sys.fn_cdc_get_column_ordinal (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  返回指定列在与指定捕获实例相关联的 [更改表](../../relational-databases/system-tables/cdc-capture-instance-ct-transact-sql.md) 中显示的列序号。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sys.fn_cdc_get_column_ordinal ( 'capture_instance','column_name')  
```  
  
## <a name="arguments"></a>参数  
 **"** *capture_instance* **"**  
 捕获实例的名称，在该实例中指定列被标识为已捕获列。 *capture_instance* **sysname**。  
  
 **"** *column_name* **"**  
 要报告的列。 *column_name* **sysname**。  
  
## <a name="return-type"></a>返回类型  
 **int**  
  
## <a name="remarks"></a>备注  
 此函数用于标识变更数据捕获更新掩码内的已捕获列的序号位置。 它主要与函数 [sys.fn_cdc_is_bit_set](../../relational-databases/system-functions/sys-fn-cdc-is-bit-set-transact-sql.md) 结合使用，以便在查询变更数据时从更新掩码中提取信息。  
  
## <a name="permissions"></a>权限  
 需要对源表的所有已捕获列具有 SELECT 权限。 如果对捕获实例指定了变更数据捕获组件的数据库角色，则同时需要具有该角色的成员身份。  
  
## <a name="examples"></a>示例  
 以下示例将获取 `VacationHours` 捕获实例更新掩码中的 `HumanResources_Employee` 列的序号位置。 然后将在调用 `sys.fn_cdc_is_bit_set` 时使用该值以从返回的更新掩码中提取信息。  
  
```  
USE AdventureWorks2012;  
GO  
DECLARE @from_lsn binary(10), @to_lsn binary(10),  @VacationHoursOrdinal int;  
SET @from_lsn = sys.fn_cdc_get_min_lsn('HumanResources_Employee');  
SET @to_lsn = sys.fn_cdc_get_max_lsn();  
SET @VacationHoursOrdinal = sys.fn_cdc_get_column_ordinal   
    ( 'HumanResources_Employee','VacationHours');  
SELECT *, sys.fn_cdc_is_bit_set(@VacationHoursOrdinal,  
    __$update_mask) as 'VacationHours'  
FROM cdc.fn_cdc_get_net_changes_HumanResources_Employee  
    ( @from_lsn, @to_lsn, 'all with mask');  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [变更数据捕获函数 (Transact-SQL)](../../relational-databases/system-functions/change-data-capture-functions-transact-sql.md)   
 [关于变更数据捕获 (SQL Server)](../../relational-databases/track-changes/about-change-data-capture-sql-server.md)   
 [sys.sp_cdc_help_change_data_capture &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-help-change-data-capture-transact-sql.md)   
 [sys.sp_cdc_get_captured_columns &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-get-captured-columns-transact-sql.md)   
 [sys.fn_cdc_is_bit_set &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-cdc-is-bit-set-transact-sql.md)  
  
  
