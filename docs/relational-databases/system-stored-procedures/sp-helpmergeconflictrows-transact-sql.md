---
description: sp_helpmergeconflictrows (Transact-SQL)
title: sp_helpmergeconflictrows (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_helpmergeconflictrows_TSQL
- sp_helpmergeconflictrows
helpviewer_keywords:
- sp_helpmergeconflictrows
ms.assetid: 131395a5-cb18-4795-a7ae-fa09d8ff347f
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 4aeb1205f3d929eed7b2717ff388d8e03891d9e3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99179254"
---
# <a name="sp_helpmergeconflictrows-transact-sql"></a>sp_helpmergeconflictrows (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  返回指定冲突表中的行。 此存储过程在存储冲突表的计算机上运行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_helpmergeconflictrows [ [ @publication = ] 'publication' ]  
        , [ @conflict_table = ] 'conflict_table'  
    [ , [ @publisher = ] 'publisher' ]   
    [ , [ @publisher_db = ] 'publsher_db' ]   
    [ , [ @logical_record_conflicts = ] logical_record_conflicts ]  
```  
  
## <a name="arguments"></a>参数  
`[ @publication = ] 'publication'` 发布的名称。 *发布* 为 **sysname**，默认值为 **%** 。 如果指定了发布，将返回由该发布限定的所有冲突。 例如，如果 " **MSmerge_conflict_Customers** " 表具有 " **WA** " 和 " **CA** " 发布的冲突行，则传入发布名称 **CA** 会检索与 **CA** 发布相关的冲突。  
  
`[ @conflict_table = ] 'conflict_table'` 冲突表的名称。 *conflict_table* **sysname**，无默认值。 在 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 和更高版本中，冲突表使用 **MSmerge_conflict \_ _发布 \__** 的格式名称进行命名，其中每个已发布项目对应一个表。  
  
`[ @publisher = ] 'publisher'` 发布服务器的名称。 *发布服务器* 的 **sysname**，默认值为 NULL。  
  
`[ @publisher_db = ] 'publisher_db'` 发布服务器数据库的名称。*publisher_db* 的默认值为 **sysname**，默认值为 NULL。  
  
`[ @logical_record_conflicts = ] logical_record_conflicts` 指示结果集是否包含有关逻辑记录冲突的信息。 *logical_record_conflicts* 的值为 **int**，默认值为0。 **1** 表示返回逻辑记录冲突信息。  
  
## <a name="result-sets"></a>结果集  
 **sp_helpmergeconflictrows** 返回由基表结构和这些附加列组成的结果集。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**origin_datasource**|**varchar(255)**|冲突的起源。|  
|**conflict_type**|**int**|表示冲突类型的代码：<br /><br /> **1** = 更新冲突：在行级别上检测到冲突。<br /><br /> **2** = 列更新冲突：在列级别上检测到冲突。<br /><br /> **3** = 更新删除 wins 冲突：删除入选冲突。<br /><br /> **4** = 更新 Wins 删除冲突：此表中记录了丢失冲突的已删除 rowguid。<br /><br /> **5** = 上载插入失败：无法在发布服务器中应用来自订阅服务器的插入。<br /><br /> **6** = 下载插入失败：无法在订阅服务器上应用从发布服务器进行的插入。<br /><br /> **7** = 上载删除失败：无法将订阅服务器上的删除内容上载到发布服务器。<br /><br /> **8** = 下载删除失败：无法将在发布服务器上删除操作下载到订阅服务器。<br /><br /> **9** = 上载更新失败：订阅服务器上的更新无法在发布服务器上应用。<br /><br /> **10** = 下载更新失败：发布服务器上的更新无法应用于订阅服务器。<br /><br /> **12** = 逻辑记录更新 Wins 删除：此表中记录了丢失冲突的已删除逻辑记录。<br /><br /> **13** = 逻辑记录冲突插入更新：插入到逻辑记录与更新冲突。<br /><br /> **14** = 逻辑记录删除 Wins 更新冲突：此表中记录了丢失冲突的更新逻辑记录。|  
|**reason_code**|**int**|与上下文相关的错误代码。|  
|**reason_text**|**varchar (720)**|与上下文相关的错误说明。|  
|**pubid**|**uniqueidentifier**|发布标识符。|  
|**MSrepl_create_time**|**datetime**|添加冲突信息的时间。|  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 **sp_helpmergeconflictrows** 用于合并复制。  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员、 **db_owner** 固定数据库角色的成员和分发数据库中的 **replmonitor** 角色才能执行 **sp_helpmergeconflictrows**。  
  
## <a name="see-also"></a>另请参阅  
 [查看 &#40;复制 Transact-sql 编程的合并发布的冲突信息&#41;](../replication/view-and-resolve-data-conflicts-for-merge-publications.md)   
 [复制存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
