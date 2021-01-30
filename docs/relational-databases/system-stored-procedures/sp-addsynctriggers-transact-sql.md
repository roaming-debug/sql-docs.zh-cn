---
description: sp_addsynctriggers (Transact-SQL)
title: sp_addsynctriggers (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_addsynctriggers_TSQL
- sp_addsynctriggers
helpviewer_keywords:
- sp_addsynctriggers
ms.assetid: e37d0c3b-19bf-4719-9535-96ba361372b3
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 00782a900c02976c06f2cfcdf6c9184d90e8059d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211944"
---
# <a name="sp_addsynctriggers-transact-sql"></a>sp_addsynctriggers (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  在订阅服务器上创建与所有类型的可更新订阅（立即、排队和将排队更新作为故障转移的立即更新）一起使用的触发器。 此存储过程在订阅服务器的订阅数据库中执行。  
  
> [!IMPORTANT]  
>  应使用 [sp_script_synctran_commands](../../relational-databases/system-stored-procedures/sp-script-synctran-commands-transact-sql.md) 过程，而不是 **sp_addsynctrigger**。 [sp_script_synctran_commands](../../relational-databases/system-stored-procedures/sp-script-synctran-commands-transact-sql.md) 生成包含 **sp_addsynctrigger** 调用的脚本。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_addsynctriggers [ @sub_table = ] 'sub_table'  
        , [ @sub_table_owner = ] 'sub_table_owner'  
        , [ @publisher = ] 'publisher'  
        , [ @publisher_db = ] 'publisher_db'  
        , [ @publication = ] 'publication'   
        , [ @ins_proc = ] 'ins_proc'   
        , [ @upd_proc = ] 'upd_proc'   
        , [ @del_proc = ] 'del_proc'   
        , [ @cftproc = ] 'cftproc'  
        , [ @proc_owner = ] 'proc_owner'  
    [ , [ @identity_col = ] 'identity_col' ]  
    [ , [ @ts_col = ] 'timestamp_col' ]  
    [ , [ @filter_clause = ] 'filter_clause' ]   
        , [ @primary_key_bitmap = ] 'primary_key_bitmap'  
    [ , [ @identity_support = ] identity_support ]  
    [ , [ @independent_agent = ] independent_agent ]  
        , [ @distributor = ] 'distributor'   
    [ , [ @pubversion = ] pubversion  
```  
  
## <a name="arguments"></a>参数  
`[ @sub_table = ] 'sub_table'` 订阅服务器表的名称。 *sub_table* **sysname**，无默认值。  
  
`[ @sub_table_owner = ] 'sub_table_owner'` 订阅服务器表的所有者的名称。 *sub_table_owner* **sysname**，无默认值。  
  
`[ @publisher = ] 'publisher'` 发布服务器的名称。 *发布服务器* 的 **sysname**，无默认值。  
  
`[ @publisher_db = ] 'publisher_db'` 发布服务器数据库的名称。 *publisher_db* **sysname**，无默认值。 如果为 NULL，则使用当前数据库。  
  
`[ @publication = ] 'publication'` 发布的名称。 *发布* 为 **sysname**，无默认值。  
  
`[ @ins_proc = ] 'ins_proc'` 在发布服务器上支持同步事务插入的存储过程的名称。 *ins_proc* **sysname**，无默认值。  
  
`[ @upd_proc = ] 'upd_proc'` 在发布服务器上支持同步事务更新的存储过程的名称。 *ins_proc* **sysname**，无默认值。  
  
`[ @del_proc = ] 'del_proc'` 在发布服务器上支持同步事务删除的存储过程的名称。 *ins_proc* **sysname**，无默认值。  
  
`[ @cftproc = ] 'cftproc'` 允许排队更新的发布使用的自动生成过程的名称。 *cftproc* 的值为 **sysname**，无默认值。 对于允许立即更新的发布，该值为 NULL。 此参数应用于允许排队更新（排队更新和将排队更新作为故障转移的立即更新）的发布。  
  
`[ @proc_owner = ] 'proc_owner'` 指定发布服务器中的用户帐户，这些存储过程用于更新发布 (排队和/或立即) 。 *proc_owner* **sysname** ，无默认值。  
  
`[ @identity_col = ] 'identity_col'` 发布服务器上标识列的名称。 *identity_col* 的默认值为 **sysname**，默认值为 NULL。  
  
`[ @ts_col = ] 'timestamp_col'` 发布服务器上 **timestamp** 列的名称。 *timestamp_col* 的默认值为 **sysname**，默认值为 NULL。  
  
`[ @filter_clause = ] 'filter_clause'` 用于定义水平筛选器的) 子句 (的限制。 当输入限制子句时，将省略关键字 WHERE。 *filter_clause* 为 **nvarchar (4000)**，默认值为 NULL。  
  
`[ @primary_key_bitmap = ] 'primary_key_bitmap'` 表中主键列的位图。 *primary_key_bitmap* 为 **varbinary (4000)**，无默认值。  
  
`[ @identity_support = ] identity_support` 使用排队更新时启用和禁用自动标识范围处理。 *identity_support* 为一个 **位**，默认值为 **0**。 **0** 表示没有标识范围支持， **1** 启用自动标识范围处理。  
  
`[ @independent_agent = ] independent_agent` 指示是否存在单个分发代理 (该发布的独立代理) ，或每个发布数据库和订阅数据库对 (共享代理) 的一个分发代理。 该值反映了发布服务器上所定义发布的 independent_agent 属性值。 *independent_agent* 是一位，默认值为 **0**。 如果为 **0**，则代理是共享代理。 如果为 **1**，则代理是独立的代理。  
  
`[ @distributor = ] 'distributor'` 分发服务器的名称。 *分发服务器* 的 **sysname**，无默认值。  
  
`[ @pubversion = ] pubversion` 指示发布服务器的版本。 *pubversion* 的值为 **int**，默认值为1。 **1** 表示发布服务器的版本为 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] Service Pack 2 或更早版本; **2** 表示发布服务器为 [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] Service PACK 3 (SP3) 或更高版本。 当发布服务器版本为 SP3 或更高版本时，pubversion 必须显式设置为 **2** [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] 。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 **sp_addsynctriggers** 由分发代理作为订阅初始化的一部分使用。 此存储过程通常不能由用户运行，但如果用户需要手动设置非同步订阅，则此过程很有用。  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员或 **db_owner** 固定数据库角色的成员才能执行 **sp_addsynctriggers**。  
  
## <a name="see-also"></a>另请参阅  
 [Updatable Subscriptions for Transactional Replication](../../relational-databases/replication/transactional/updatable-subscriptions-for-transactional-replication.md)   
 [sp_script_synctran_commands &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-script-synctran-commands-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
