---
description: sys.fn_cdc_map_time_to_lsn (Transact-SQL)
title: sys.fn_cdc_map_time_to_lsn (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.fn_cdc_map_time_to_lsn
- fn_cdc_map_time_to_lsn_TSQL
- sys.fn_cdc_map_time_to_lsn_TSQL
- fn_cdc_map_time_to_lsn
dev_langs:
- TSQL
helpviewer_keywords:
- fn_cdc_map_time_to_lsn
- sys.fn_cdc_map_time_to_lsn
ms.assetid: 6feb051d-77ae-4c93-818a-849fe518d1d4
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 38f07d3d3c46a46bc18f84d54b14809c201ee219
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094955"
---
# <a name="sysfn_cdc_map_time_to_lsn-transact-sql"></a>sys.fn_cdc_map_time_to_lsn (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  返回 (LSN 的日志序列号) 指定时间内 [cdc.lsn_time_mapping](../../relational-databases/system-tables/cdc-lsn-time-mapping-transact-sql.md)系统表中 **start_lsn** 列的值。 您可以使用此函数将日期时间范围系统地映射到变更数据捕获枚举函数所需的基于 LSN 的范围， [cdc.fn_cdc_get_all_changes_<capture_instance>](../../relational-databases/system-functions/cdc-fn-cdc-get-all-changes-capture-instance-transact-sql.md) 并 [cdc.fn_cdc_get_net_changes_ ](../../relational-databases/system-functions/cdc-fn-cdc-get-net-changes-capture-instance-transact-sql.md)<capture_instance>返回该范围内的数据更改。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sys.fn_cdc_map_time_to_lsn ( '<relational_operator>', tracking_time )  
  
<relational_operator> ::=  
{  largest less than  
 | largest less than or equal  
 | smallest greater than  
 | smallest greater than or equal  
}  
```  
  
## <a name="arguments"></a>参数  
 **"**<relational_operator>**"** {小于 \| 或等于小于或等于最小大于 \| \| 或等于的最大值}  
 用于标识 **cdc.lsn_time_mapping** 表中的不同 LSN 值以及与 *tracking_time* 值相比满足关系的关联 **tran_end_time** 。  
  
 *relational_operator* 的 **(30) 为 nvarchar**。  
  
 *tracking_time*  
 要进行匹配的日期时间值。 *tracking_time* 为 **日期时间**。  
  
## <a name="return-type"></a>返回类型  
 **binary(10)**  
  
## <a name="remarks"></a>备注  
 若要了解如何使用 **sys.fn_cdc_map_time_lsn** 将日期时间范围映射到 lsn 范围，请考虑以下方案。 假定使用者希望每日提取更改数据。 也就是说，使用者只需要给定日午夜之前（包括午夜）的更改。 此时间范围的下限应为无限接近前一天午夜的时间点（但不包括前一天午夜）。 上限应为给定日的午夜（包括午夜）。 下面的示例演示如何使用函数 **sys.fn_cdc_map_time_to_lsn** 来将此基于时间的范围系统地映射到变更数据捕获枚举函数所需的基于 lsn 的范围，以返回该范围内的所有更改。  
  
 `DECLARE @begin_time datetime, @end_time datetime, @begin_lsn binary(10), @end_lsn binary(10);`  
  
 `SET @begin_time = '2007-01-01 12:00:00.000';`  
  
 `SET @end_time = '2007-01-02 12:00:00.000';`  
  
 `SELECT @begin_lsn = sys.fn_cdc_map_time_to_lsn('smallest greater than', @begin_time);`  
  
 `SELECT @end_lsn = sys.fn_cdc_map_time_to_lsn('largest less than or equal', @end_time);`  
  
 `SELECT * FROM cdc.fn_cdc_get_net_changes_HR_Department(@begin_lsn, @end_lsn, 'all` `');`  
  
 关系运算符 '`smallest greater than`' 用于将更改限制为在前一天的午夜后发生的更改。 如果具有不同 LSN 值的多个条目共享 [cdc.lsn_time_mapping](../../relational-databases/system-tables/cdc-lsn-time-mapping-transact-sql.md)表中标识为下限的 **tran_end_time** 值，该函数将返回最小的 lsn，以确保包含所有条目。 对于上限，关系运算符 " `largest less than or equal to` " 用于确保该范围包含当天的所有条目，其中包括除午夜之外的所有条目作为 **tran_end_time** 值。 如果具有不同 LSN 值的多个条目共享标识为上限的 **tran_end_time** 值，则该函数将返回最大的 lsn，以确保包含所有条目。  
  
## <a name="permissions"></a>权限  
 要求 **公共** 角色具有成员身份。  
  
## <a name="examples"></a>示例  
 下面的示例使用 `sys.fn_cdc_map_time_lsn` 函数来确定 [cdc.lsn_time_mapping](../../relational-databases/system-tables/cdc-lsn-time-mapping-transact-sql.md) 表中是否存在一个大于或等于午夜 **tran_end_time** 值的行。 例如，可以用此查询来确定捕获进程是否已处理完截至前一天午夜提交的更改，以便接下来可以提取当天的更改数据。  
  
```  
DECLARE @extraction_time datetime, @lsn binary(10);  
SET @extraction_time = '2007-01-01 12:00:00.000';  
SELECT @lsn = sys.fn_cdc_map_time_to_lsn ('smallest greater than or equal', @extraction_time);  
IF @lsn IS NOT NULL  
BEGIN  
<some action>  
END  
```  
  
## <a name="see-also"></a>另请参阅  
 [cdc.lsn_time_mapping &#40;Transact-sql&#41;](../../relational-databases/system-tables/cdc-lsn-time-mapping-transact-sql.md)   
 [sys.fn_cdc_map_lsn_to_time &#40;Transact-sql&#41;](../../relational-databases/system-functions/sys-fn-cdc-map-lsn-to-time-transact-sql.md)   
 [&#62; &#40;Transact-sql&#41;cdc.fn_cdc_get_net_changes_&#60;capture_instance ](../../relational-databases/system-functions/cdc-fn-cdc-get-net-changes-capture-instance-transact-sql.md)   
 [cdc.fn_cdc_get_all_changes_&#60;capture_instance&#62;  &#40;Transact-SQL&#41;](../../relational-databases/system-functions/cdc-fn-cdc-get-all-changes-capture-instance-transact-sql.md)  
  
  
