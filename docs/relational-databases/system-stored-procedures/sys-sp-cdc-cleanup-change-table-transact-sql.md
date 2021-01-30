---
description: sys.sp_cdc_cleanup_change_table (Transact-SQL)
title: sys.sp_cdc_cleanup_change_table (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_cdc_cleanup_change_table
- sp_cdc_cleanup_change_table_TSQL
- sys.sp_cdc_cleanup_change_table
- sys.sp_cdc_cleanup_change_table_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.sp_cdc_cleanup_change_tables
- sp_cdc_cleanup_change_tables
ms.assetid: 02295794-397d-4445-a3e3-971b25e7068d
author: markingmyname
ms.author: maghan
ms.openlocfilehash: fc6ca4964beff8adc8f32598948964d597076804
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210749"
---
# <a name="syssp_cdc_cleanup_change_table-transact-sql"></a>sys.sp_cdc_cleanup_change_table (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  基于指定的 *low_water_mark* 值从当前数据库的更改表中删除行。 此存储过程是为需要直接管理更改表清除进程的用户提供的。 但是，由于此过程会影响更改表中数据的所有使用者，因而应多加小心。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sys.sp_cdc_cleanup_change_table   
  [ @capture_instance = ] 'capture_instance',   
  [ @low_water_mark = ] low_water_mark ,  
  [ @threshold = ]'delete threshold'  
```  
  
## <a name="arguments"></a>参数  
 [ @capture_instance =] "*capture_instance*"  
 与更改表关联的捕获实例的名称。 *capture_instance* **sysname**，无默认值，且不能为 NULL。  
  
 *capture_instance* 必须命名当前数据库中存在的捕获实例。  
  
 [ @low_water_mark =] *low_water_mark*  
 是 (LSN) 的日志序列号，将用作 *捕获实例* 的新低水印。 *low_water_mark* 是 **二进制 (10)**，无默认值。  
  
 如果该值为非空值，则它必须显示为 [cdc.lsn_time_mapping](../../relational-databases/system-tables/cdc-lsn-time-mapping-transact-sql.md) 表中当前项的 start_lsn 值。 如果 cdc.lsn_time_mapping 中的其他条目共享与新的低水印所标识的条目相同的提交时间，则选择与该组条目关联的最小 LSN 作为低水印。  
  
 如果将值显式设置为 NULL，则 *捕获实例* 的当前 *低水印* 用于定义清理操作的上限。  
  
 [ @threshold =] '*删除阈值*'  
 清除时可以使用一条语句删除的删除项的最大数量。 *delete_threshold* 为 **bigint**，默认值为5000。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="result-sets"></a>结果集  
 无  
  
## <a name="remarks"></a>备注  
 sys.sp_cdc_cleanup_change_table 执行以下操作：  
  
1.  如果 @low_water_mark 参数不为 NULL，则会将 *捕获实例* 的 start_lsn 的值设置为新的 *低水位线*。  
  
    > [!NOTE]  
    >  新的低水印可能不是在存储过程调用中指定的低水印。 如果 cdc.lsn_time_mapping 表中的其他条目共享相同的提交时间，则在这组条目中表示的最小 start_lsn 将被选为调整后的低水印。 如果 @low_water_mark 参数为 NULL 或者当前低水印大于新的低水印，则捕获实例的 start_lsn 值将保持不变。  
  
2.  然后删除其 __$start_lsn 值小于低水印的更改表条目。 删除阈值用于限制在单个事务中删除的行数。 如果未成功删除条目，将进行报告，但不影响可能已基于此调用而对捕获实例低水印进行的任何更改。  

 在以下环境中使用 sys.sp_cdc_cleanup_change_table：  
  
-   清理代理作业报告删除失败。  
  
     管理员可以运行此存储过程以显式重试失败的操作。 若要对给定的捕获实例重试清理，请执行 sys.sp_cdc_cleanup_change_table，并为 @low_water_mark 参数指定 NULL。  
  
-   由清理代理作业使用的基于保留期的简单策略不能满足要求。  
  
     因为此存储过程对单个捕获实例执行清理，因此可以用它生成自定义清理策略，以便根据各个具体的捕获实例调整清理规则。  
  
## <a name="permissions"></a>权限  
 要求具有 db_owner 固定数据库角色中的成员资格。  
  
## <a name="see-also"></a>另请参阅  
 [&#62;  &#40;Transact-sql&#41;cdc.fn_cdc_get_all_changes_&#60;capture_instance ](../../relational-databases/system-functions/cdc-fn-cdc-get-all-changes-capture-instance-transact-sql.md)   
 [sys.fn_cdc_get_min_lsn &#40;Transact-sql&#41;](../../relational-databases/system-functions/sys-fn-cdc-get-min-lsn-transact-sql.md)   
 [sys.fn_cdc_increment_lsn &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-cdc-increment-lsn-transact-sql.md)  
  
  
