---
description: sp_syscollector_create_collection_set (Transact-SQL)
title: sp_syscollector_create_collection_set (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_syscollector_create_collection_set_TSQL
- sp_syscollector_create_collection_set
dev_langs:
- TSQL
helpviewer_keywords:
- data collector [SQL Server], stored procedures
- sp_syscollector_create_collection_set
ms.assetid: 69e9ff0f-c409-43fc-89f6-40c3974e972c
author: markingmyname
ms.author: maghan
ms.openlocfilehash: de9ee3331621df81739a9f808bbd63ab42c52491
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205075"
---
# <a name="sp_syscollector_create_collection_set-transact-sql"></a>sp_syscollector_create_collection_set (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  创建新的收集组。 您可以使用此存储过程为数据收集创建一个自定义收集组。  
  
> [!WARNING]  
>  在配置为代理的 Windows 帐户是非交互用户或者是尚未登录的交互用户时，配置文件目录将不存在，并且临时目录的创建将失败。 因此，如果您正在域控制器上使用代理帐户，则必须指定已使用至少一次的交互帐户，以便确保创建了配置文件目录。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_syscollector_create_collection_set   
      [ @name = ] 'name'  
    , [ [ @target = ] 'target' ]  
    , [ [ @collection_mode = ] collection_mode ]  
    , [ [ @days_until_expiration = ] days_until_expiration ]  
    , [ [ @proxy_id = ] proxy_id ]  
    , [ [ @proxy_name = ] 'proxy_name' ]  
    , [ [ @schedule_uid = ] 'schedule_uid' ]  
    , [ [ @schedule_name = ] 'schedule_name' ]  
    , [ [ @logging_level = ] logging_level ]  
    , [ [ @description = ] 'description' ]  
    , [ @collection_set_id = ] collection_set_id OUTPUT   
    , [ [ @collection_set_uid = ] 'collection_set_uid' OUTPUT ]  
```  
  
## <a name="arguments"></a>参数  
`[ @name = ] 'name'` 收集组的名称。 *名称* 为 **sysname** ，不能为空字符串或 NULL。  
  
 *名称* 必须是唯一的。 有关当前收集组名称的列表，请查询 syscollector_collection_sets 系统视图。  
  
`[ @target = ] 'target'` 保留供将来使用。 *名称* 为 **nvarchar (128)** ，默认值为 NULL。  
  
`[ @collection_mode = ] collection_mode` 指定收集和存储数据的方式。 *collection_mode* 为 **smallint** ，可以具有以下值之一：  
  
 0 - 缓存模式。 数据收集和上载分别位于各自的计划中。 为连续收集指定缓存模式。  
  
 1 - 非缓存模式。 数据的收集和上载位于同一个计划中。 为临时收集或快照收集指定非缓存模式。  
  
 *Collection_mode* 的默认值为0。 如果 *collection_mode* 为0，则必须指定 *schedule_uid* 或 *schedule_name* 。  
  
`[ @days_until_expiration = ] days_until_expiration` 收集的数据保存在管理数据仓库中的天数。 *days_until_expiration* 为 **smallint** ，默认值为 730 (两年) 。 *days_until_expiration* 必须是0或正整数。  
  
`[ @proxy_id = ] proxy_id` 代理的代理帐户的唯一标识符 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 *proxy_id* 为 **int** ，默认值为 NULL。 如果已指定，则 *proxy_name* 必须为 NULL。 若要获取 *proxy_id*，请查询 sysproxies 系统表。 dc_admin 固定数据库角色必须拥有访问代理的权限。 有关详细信息，请参阅 [创建 SQL Server 代理代理](../../ssms/agent/create-a-sql-server-agent-proxy.md)。  
  
`[ @proxy_name = ] 'proxy_name'` 代理帐户的名称。 *proxy_name* 的默认值为 NULL，则为 **sysname** 。 如果已指定，则 *proxy_id* 必须为 NULL。 若要获取 *proxy_name*，请查询 sysproxies 系统表。  
  
`[ @schedule_uid = ] 'schedule_uid'` 指向计划的 GUID。 *schedule_uid* 为 **uniqueidentifier** ，默认值为 NULL。 如果已指定，则 *schedule_name* 必须为 NULL。 若要获取 *schedule_uid*，请查询 sysschedules 引用系统表。  
  
 如果 *collection_mode* 设置为0，则必须指定 *schedule_uid* 或 *schedule_name* 。 如果 *collection_mode* 设置为1，则将忽略指定的 *schedule_uid* 或 *schedule_name* 。  
  
`[ @schedule_name = ] 'schedule_name'` 计划的名称。 *schedule_name* 的默认值为 NULL，则为 **sysname** 。 如果已指定，则 *schedule_uid* 必须为 NULL。 若要获取 *schedule_name*，请查询 sysschedules 引用系统表。  
  
`[ @logging_level = ] logging_level` 日志记录级别。 *logging_level* 为 **smallint** ，并具有以下值之一：  
  
 0 - 记录执行信息和跟踪以下内容的 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 事件：  
  
-   启动/停止收集组  
  
-   启动/停止包  
  
-   错误信息  
  
 1 - 级别 0 日志记录和：  
  
-   执行统计信息  
  
-   连续运行收集的进度  
  
-   来自 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 的警告事件  
  
 2 - 级别 1 日志记录和来自 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 的详细事件信息  
  
 *Logging_level* 的默认值为1。  
  
`[ @description = ] 'description'` 收集组的说明。 *说明* 为 **nvarchar (4000)** ，默认值为 NULL。  
  
`[ @collection_set_id = ] collection_set_id` 收集组的唯一本地标识符。 *collection_set_id* 是包含 OUTPUT 的 **int** ，并且是必需的。  
  
`[ @collection_set_uid = ] 'collection_set_uid'` 收集组的 GUID。 *collection_set_uid* 是具有默认值 NULL 的带有 OUTPUT 的 **uniqueidentifier** 。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 sp_syscollector_create_collection_set 必须在 msdb 系统数据库的上下文中运行。  
  
## <a name="permissions"></a>权限  
 需要具有 dc_admin（拥有 EXECUTE 权限）固定数据库角色的成员身份才能执行此过程。  
  
## <a name="examples"></a>示例  
  
### <a name="a-creating-a-collection-set-by-using-default-values"></a>A. 使用默认值创建收集组  
 下面的示例通过仅指定所需参数来创建收集组。 `@collection_mode` 不是必需的，但默认收集模式（缓存）要求指定计划 ID 或计划名称。  
  
```  
USE msdb;  
GO  
DECLARE @collection_set_id int;  
EXECUTE dbo.sp_syscollector_create_collection_set  
    @name = N'Simple collection set test 1',  
    @description = N'This is a test collection set that runs in non-cached mode.',  
    @collection_mode = 1,  
    @collection_set_id = @collection_set_id OUTPUT;  
GO  
```  
  
### <a name="b-creating-a-collection-set-by-using-specified-values"></a>B. 使用指定值创建收集组  
 下面的示例通过指定多个参数的值来创建收集组。  
  
```  
USE msdb;  
GO  
DECLARE @collection_set_id int;  
DECLARE @collection_set_uid uniqueidentifier;  
SET @collection_set_uid = NEWID();  
EXEC dbo.sp_syscollector_create_collection_set  
    @name = N'Simple collection set test 2',  
    @collection_mode = 0,  
    @days_until_expiration = 365,  
    @description = N'This is a test collection set that runs in cached mode.',  
    @logging_level = 2,  
    @schedule_name = N'CollectorSchedule_Every_30min',  
    @collection_set_id = @collection_set_id OUTPUT,  
    @collection_set_uid = @collection_set_uid OUTPUT;  
```  
  
## <a name="see-also"></a>另请参阅  
 [数据收集](../../relational-databases/data-collection/data-collection.md)   
 [创建使用一般 T-SQL 查询收集器类型的自定义收集组 (Transact-SQL)](../../relational-databases/data-collection/create-custom-collection-set-generic-t-sql-query-collector-type.md)   
 [数据收集器存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/data-collector-stored-procedures-transact-sql.md)   
 [syscollector_collection_sets (Transact-SQL)](../../relational-databases/system-catalog-views/syscollector-collection-sets-transact-sql.md)  
  
  
