---
description: sp_syscollector_update_collection_set (Transact-SQL)
title: sp_syscollector_update_collection_set (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_syscollector_update_collection_set_TSQL
- sp_syscollector_update_collection_set
dev_langs:
- TSQL
helpviewer_keywords:
- sp_syscollector_update_collection_set
- data collector [SQL Server], stored procedures
ms.assetid: 2dccc3cd-0e93-4e3e-a4e5-8fe89b31bd63
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 207d5b2f02bce0a3c9a9e89cdccdb5fe80790f20
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99161885"
---
# <a name="sp_syscollector_update_collection_set-transact-sql"></a>sp_syscollector_update_collection_set (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  用于修改用户定义的收集组的属性或重命名用户定义的收集组。  
  
> [!WARNING]  
>  在配置为代理的 Windows 帐户是非交互用户或者是尚未登录的交互用户时，配置文件目录将不存在，并且临时目录的创建将失败。 因此，如果您正在域控制器上使用代理帐户，则必须指定已使用至少一次的交互帐户，以便确保创建了配置文件目录。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_syscollector_update_collection_set   
    [ [ @collection_set_id = ] collection_set_id ]  
    , [ [ @name = ] 'name' ]  
    , [ [ @new_name = ] 'new_name' ]  
    , [ [ @target = ] 'target' ]  
    , [ [ @collection_mode = ] collection_mode ]  
    , [ [ @days_until_expiration = ] days_until_expiration ]  
    , [ [ @proxy_id = ] proxy_id ]  
    , [ [ @proxy_name = ] 'proxy_name' ]  
    ,[ [ @schedule_uid = ] 'schedule_uid' ]  
    ,[ [ @schedule_name = ] 'schedule_uid' ]  
    , [ [ @logging_level = ] logging_level ]  
    , [ [ @description = ] 'description' ]  
```  
  
## <a name="arguments"></a>参数  
`[ @collection_set_id = ] collection_set_id` 收集组的唯一本地标识符。 *collection_set_id* 为 **int** ，并且 *name* 为 NULL 时必须具有值。  
  
`[ @name = ] 'name'` 收集组的名称。 *名称* 为 **sysname** ，并且 *collection_set_id* 为 NULL 时必须具有值。  
  
`[ @new_name = ] 'new_name'` 收集组的新名称。 *new_name* 为 **sysname**，如果使用，则不能为空字符串。 *new_name* 必须是唯一的。 有关当前收集组名称的列表，请查询 syscollector_collection_sets 系统视图。  
  
`[ @target = ] 'target'` 保留供将来使用。  
  
`[ @collection_mode = ] collection_mode` 要使用的数据集合的类型。 *collection_mode* 为 **smallint** ，可以具有以下值之一：  
  
 0 - 缓存模式。 数据收集和上载分别位于各自的计划中。 为连续收集指定缓存模式。  
  
 1 - 非缓存模式。 数据的收集和上载位于同一个计划中。 为临时收集或快照收集指定非缓存模式。  
  
 如果从非缓存模式更改为缓存模式 (0) ，则还必须指定 *schedule_uid* 或 *schedule_name*。  
  
`[ @days_until_expiration = ] days_until_expiration` 收集的数据保存在管理数据仓库中的天数。 *days_until_expiration* 为 **smallint**。 *days_until_expiration* 必须是0或正整数。  
  
`[ @proxy_id = ] proxy_id` 代理的代理帐户的唯一标识符 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 *proxy_id* 是 **int**。  
  
`[ @proxy_name = ] 'proxy_name'` 代理的名称。 *proxy_name* 是 **sysname** ，可为 null。  
  
`[ @schedule_uid = ] 'schedule_uid'` 指向计划的 GUID。 *schedule_uid* 是 **uniqueidentifier**。  
  
 若要获取 *schedule_uid*，请查询 sysschedules 引用系统表。  
  
 如果 *collection_mode* 设置为0，则必须指定 *schedule_uid* 或 *schedule_name* 。 如果 *collection_mode* 设置为1，则将忽略指定的 *schedule_uid* 或 *schedule_name* 。  
  
`[ @schedule_name = ] 'schedule_name'` 计划的名称。 *schedule_name* 是 **sysname** ，可为 null。 如果已指定，则 *schedule_uid* 必须为 NULL。 若要获取 *schedule_name*，请查询 sysschedules 引用系统表。  
  
`[ @logging_level = ] logging_level` 日志记录级别。 *logging_level* 为 **smallint** ，并具有以下值之一：  
  
 0-记录执行信息和 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 跟踪事件：  
  
-   启动/停止收集组  
  
-   启动/停止包  
  
-   错误信息  
  
 1级日志记录和：  
  
-   执行统计信息  
  
-   连续运行收集的进度  
  
-   来自 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 的警告事件  
  
 2 - 级别 1 日志记录和来自 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 的详细事件信息。  
  
 *Logging_level* 的默认值为1。  
  
`[ @description = ] 'description'` 收集组的说明。 *描述* 为 **nvarchar (4000)**。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 sp_syscollector_update_collection_set 必须在 msdb 系统数据库的上下文中运行。  
  
 *Collection_set_id* 或 *name* 必须具有值，两者都不能为 NULL。 若要获取这些值，请查询 syscollector_collection_sets 系统视图。  
  
 如果收集组正在运行，则只能更新 *schedule_uid* 和 *说明*。 若要停止收集组，请使用 [sp_syscollector_stop_collection_set](../../relational-databases/system-stored-procedures/sp-syscollector-stop-collection-set-transact-sql.md)。  
  
## <a name="permissions"></a>权限  
 要求具有 dc_admin 或 dc_operator（拥有 EXECUTE 权限）固定数据库角色的成员身份才能执行此过程。 尽管 dc_operator 可以运行此存储过程，但是此角色的成员在其属性更改权限方面受到限制。 下列属性只能由 dc_admin 更改：  
  
-   @new_name  
  
-   @target  
  
-   @proxy_id  
  
-   @description  
  
-   @collection_mode  
  
-   @days_until_expiration  
  
## <a name="examples"></a>示例  
  
### <a name="a-renaming-a-collection-set"></a>A. 重命名收集组  
 下面的示例重命名用户定义的收集组。  
  
```  
USE msdb;  
GO  
EXECUTE dbo.sp_syscollector_update_collection_set  
@name = N'Simple collection set test 1',  
@new_name = N'Collection set test 1 in cached mode';  
GO  
```  
  
### <a name="b-changing-the-collection-mode-from-non-cached-to-cached"></a>B. 将收集模式从非缓存模式更改为缓存模式  
 下面的示例将收集模式从非缓存模式更改为缓存模式。 此更改需要您指定计划 ID 或计划名称。  
  
```  
USE msdb;  
GO  
EXECUTE dbo.sp_syscollector_update_collection_set  
@name = N'Collection set test 1 in cached mode',  
@collection_mode = 0,  
@schedule_uid = 'C7022AF3-51B8-4011-B159-64C47C88FF70';  
-- alternatively, use @schedule_name.  
-- @schedule_name = N'CollectorSchedule_Every_15min;  
GO  
```  
  
### <a name="c-changing-other-collection-set-parameters"></a>C. 更改其他收集组参数  
 下面的示例更新名为“Simple collection set test 2”的收集组的各种属性。  
  
```  
USE msdb;  
GO  
EXEC dbo.sp_syscollector_update_collection_set  
@name = N'Simple collection set test 2',  
@collection_mode = 1,  
@days_until_expiration = 5,  
@description = N'This is a test collection set that runs in noncached mode.',  
@logging_level = 0;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [数据收集](../../relational-databases/data-collection/data-collection.md)   
 [syscollector_collection_sets &#40;Transact-sql&#41;](../../relational-databases/system-catalog-views/syscollector-collection-sets-transact-sql.md)   
 [dbo.sys计划 &#40;Transact-sql&#41;](../../relational-databases/system-tables/dbo-sysschedules-transact-sql.md)  
  
  
