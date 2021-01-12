---
description: sys.fn_cdc_increment_lsn (Transact-SQL)
title: sys.fn_cdc_increment_lsn (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- fn_cdc_increment_lsn_TSQL
- sys.fn_cdc_increment_lsn_TSQL
- sys.fn_cdc_increment_lsn
- fn_cdc_increment_lsn
dev_langs:
- TSQL
helpviewer_keywords:
- fn_cdc_increment_lsn
- sys.fn_cdc_increment_lsn
ms.assetid: e53b6703-358b-4c9a-912a-8f7c7331069b
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: ee5d44cbe53d31609a454eb5acbed52a53ae194e
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099653"
---
# <a name="sysfn_cdc_increment_lsn-transact-sql"></a>sys.fn_cdc_increment_lsn (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  根据指定的 LSN 返回序列中的下一个日志序列号 (LSN)。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sys.fn_cdc_increment_lsn ( lsn_value )  
```  
  
## <a name="arguments"></a>参数  
 *lsn_value*  
 LSN 值。 *lsn_value* 是 **(10) 的二进制**。  
  
## <a name="return-type"></a>返回类型  
 **binary(10)**  
  
## <a name="remarks"></a>备注  
 此函数返回的 LSN 值始终大于指定的值，并且不存在介于这两个值之间的 LSN 值。  
  
 若要系统地查询随时间变化的更改数据流，可以定期重复调用该查询函数，每次调用时指定一个新的查询间隔来限定查询中返回的更改的范围。 为帮助确保不丢失数据，通常使用前一个查询的上限来生成后一个查询的下限。 由于查询间隔是一个闭区间，因此新的下限必须大于前一个上限，但要足够小，以确保不存在 LSN 值介于此值与旧上限之间的更改。 sys.fn_cdc_increment_lsn 函数就是用来获取此值的。  
  
## <a name="permissions"></a>权限  
 要求具有公用数据库角色的成员身份。  
  
## <a name="examples"></a>示例  
 下例将前一个查询的上限保存到变量 `sys.fn_cdc_increment_lsn` 中，然后根据此上限使用 `@save_to_lsn` 来生成变更数据捕获查询的新下限值。  
  
```  
USE AdventureWorks2012;  
GO  
DECLARE @from_lsn binary(10), @to_lsn binary(10), @save_to_lsn binary(10);  
SET @save_to_lsn = <previous_upper_bound_value>;  
SET @from_lsn = sys.fn_cdc_increment_lsn(@save_to_lsn);  
SET @to_lsn = sys.fn_cdc_get_max_lsn();  
SELECT * from cdc.fn_cdc_get_all_changes_HumanResources_Employee( @from_lsn, @to_lsn, 'all' );  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [sys.fn_cdc_decrement_lsn &#40;Transact-sql&#41;](../../relational-databases/system-functions/sys-fn-cdc-decrement-lsn-transact-sql.md)   
 [&#62;  &#40;Transact-sql&#41;cdc.fn_cdc_get_all_changes_&#60;capture_instance ](../../relational-databases/system-functions/cdc-fn-cdc-get-all-changes-capture-instance-transact-sql.md)   
 [&#62; &#40;Transact-sql&#41;cdc.fn_cdc_get_net_changes_&#60;capture_instance ](../../relational-databases/system-functions/cdc-fn-cdc-get-net-changes-capture-instance-transact-sql.md)   
 [事务日志 (SQL Server)](../../relational-databases/logs/the-transaction-log-sql-server.md)   
 [关于变更数据捕获 (SQL Server)](../../relational-databases/track-changes/about-change-data-capture-sql-server.md)  
  
  
