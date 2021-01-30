---
description: sys.sp_cdc_change_job (Transact-SQL)
title: sys.sp_cdc_change_job (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.sp_cdc_change_job_TSQL
- sys.sp_cdc_change_job
- sp_cdc_change_job_TSQL
- sp_cdc_change_job
dev_langs:
- TSQL
helpviewer_keywords:
- sp_cdc_change_job
ms.assetid: ea918888-0fc5-4cc1-b301-26b2a9fbb20d
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 0dcc3a8b69e6cf8e65f96b239dd7699f796246ba
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210760"
---
# <a name="syssp_cdc_change_job-transact-sql"></a>sys.sp_cdc_change_job (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  修改当前数据库中变更数据捕获清除或捕获作业的配置。 若要查看作业的当前配置，请查询 [dbo.cdc_jobs](../../relational-databases/system-tables/dbo-cdc-jobs-transact-sql.md) 表，或使用 [sp_cdc_help_jobs](../../relational-databases/system-stored-procedures/sys-sp-cdc-help-jobs-transact-sql.md)。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sys.sp_cdc_change_job [ [ @job_type = ] 'job_type' ]  
    [ , [ @maxtrans = ] max_trans ]   
    [ , [ @maxscans = ] max_scans ]   
    [ , [ @continuous = ] continuous ]   
    [ , [ @pollinginterval = ] polling_interval ]   
    [ , [ @retention ] = retention ]   
    [ @threshold = ] 'delete threshold'  
```  
  
## <a name="arguments"></a>参数  
`[ @job_type = ] 'job_type'` 要修改的作业的类型。 *job_type* 为 **nvarchar (20)** ，默认值为 "捕获"。 有效的输入为 'capture' 和 'cleanup'。  
  
`[ @maxtrans ] = max_trans_` 每个扫描循环中要处理的最大事务数。 *max_trans* 的默认值为 **int** ，默认值为 NULL，指示未更改此参数。 如果指定值，则该值必须是一个正整数。  
  
 *max_trans* 仅对捕获作业有效。  
  
`[ @maxscans ] = max_scans_` 为了从日志中提取所有行而要执行的扫描周期的最大数目。 *max_scans* 的默认值为 **int** ，默认值为 NULL，指示未更改此参数。  
  
 *max_scan* 仅对捕获作业有效。  
  
`[ @continuous ] = continuous_` 指示捕获作业是要连续运行 (1) 还是只运行一次 (0) 。 *连续* 为 **bit** ，默认值为 NULL，指示未更改此参数。  
  
 当 *连续* = 1 时， [sp_cdc_scan](../../relational-databases/system-stored-procedures/sys-sp-cdc-scan-transact-sql.md) 作业将扫描日志并最多处理 (*max_trans* \* *max_scans*) 事务。 然后，在开始下一次日志扫描之前，会等待 *polling_interval* 中指定的秒数。  
  
 当 *连续* = 0 时， **sp_cdc_scan** 作业最多执行一次日志 *max_scans* 扫描，每次扫描时处理 *max_trans* 事务，然后退出。  
  
 如果 **\@ 连续** 的更改从1改为0，则 **\@ pollinginterval** 会自动设置为0。 将忽略为 **\@ pollinginterval** 以外的其他值指定的值。  
  
 如果省略 **\@ 连续** 或显式设置为 NULL，并且 **\@ pollinginterval** 被显式设置为大于0的值，则 **\@ 连续** 自动设置为1。  
  
 *连续* 仅对捕获作业有效。  
  
`[ @pollinginterval ] = polling_interval_` 日志扫描循环之间的秒数。 *polling_interval* 为 **bigint** ，默认值为 NULL，指示未更改此参数。  
  
 如果将 "*连续*" 设置为1，则 *polling_interval* 仅对捕获作业有效。  
  
`[ @retention ] = retention_` 更改行要在更改表中保留的分钟数。 *保留期* 为 **bigint** ，默认值为 NULL，指示未更改此参数。 最大值为 52494800（100 年）。 如果指定值，则该值必须是一个正整数。  
  
 *保留* 仅对清除作业有效。  
  
`[ @threshold = ] 'delete threshold'` 清除时可以使用单个语句删除的删除条目的最大数量。 *delete 阈值* 为 **bigint** ，默认值为 NULL，指示未更改此参数。 *delete 阈值* 仅对清除作业有效。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="result-sets"></a>结果集  
 无  
  
## <a name="remarks"></a>备注  
 如果省略参数，则不会更新 [dbo.cdc_jobs](../../relational-databases/system-tables/dbo-cdc-jobs-transact-sql.md) 表中的关联值。 如果将参数显式设置为 NULL，则视作省略此参数。  
  
 如果指定的参数对相应作业类型无效，则会导致该语句失败。  
  
 作业的更改在使用 [sp_cdc_stop_job](../../relational-databases/system-stored-procedures/sys-sp-cdc-stop-job-transact-sql.md) 停止并通过使用 [sp_cdc_start_job](../../relational-databases/system-stored-procedures/sys-sp-cdc-start-job-transact-sql.md)重启之前，不会生效。  
  
## <a name="permissions"></a>权限  
 需要 **db_owner** 固定数据库角色的成员身份。  
  
## <a name="examples"></a>示例  
  
### <a name="a-changing-a-capture-job"></a>A. 更改捕获作业  
 下面的示例 `@job_type` `@maxscans` `@maxtrans` 在数据库中更新捕获作业的、和参数 `AdventureWorks2012` 。 省略了捕获作业的其他有效参数（`@continuous` 和 `@pollinginterval`）；未修改它们的值。  
  
```  
USE AdventureWorks2012;  
GO  
EXECUTE sys.sp_cdc_change_job   
    @job_type = N'capture',  
    @maxscans = 1000,  
    @maxtrans = 15;  
GO  
```  
  
### <a name="b-changing-a-cleanup-job"></a>B. 更改清除作业  
 下例更新 `AdventureWorks2012` 数据库中的一个清除作业。 此作业类型的所有有效参数（ **\@ 阈值** 除外）均已指定。 不会修改 **\@ 阈值** 的值。  
  
```  
USE AdventureWorks2012;  
GO  
EXECUTE sys.sp_cdc_change_job   
    @job_type = N'cleanup',  
    @retention = 2880;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [dbo.cdc_jobs &#40;Transact-sql&#41;](../../relational-databases/system-tables/dbo-cdc-jobs-transact-sql.md)   
 [sys.sp_cdc_enable_table &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-enable-table-transact-sql.md)   
 [sys.sp_cdc_add_job (Transact-SQL)](../../relational-databases/system-stored-procedures/sys-sp-cdc-add-job-transact-sql.md)  
  
  
