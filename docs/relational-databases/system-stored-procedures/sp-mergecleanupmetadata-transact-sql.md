---
description: sp_mergecleanupmetadata (Transact-SQL)
title: sp_mergecleanupmetadata (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_mergecleanupmetadata_TSQL
- sp_mergecleanupmetadata
helpviewer_keywords:
- sp_mergecleanupmetadata
ms.assetid: 892f8628-4cbe-4cc3-b959-ed45ffc24064
author: markingmyname
ms.author: maghan
ms.openlocfilehash: f6fa369de984a81e42185ddedfb874c672c3b541
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185303"
---
# <a name="sp_mergecleanupmetadata-transact-sql"></a>sp_mergecleanupmetadata (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  只应在包括运行 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] Service Pack 1 之前版本的服务器的复制拓扑中使用。**sp_mergecleanupmetadata** 允许管理员清理 **MSmerge_genhistory**、 **MSmerge_contents** 和 **MSmerge_tombstone** 系统表中的元数据。 此存储过程在发布服务器上对发布数据库执行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_mergecleanupmetadata [ [ @publication = ] 'publication' ]  
    [ , [ @reinitialize_subscriber = ] 'reinitialize_subscriber' ]  
```  
  
## <a name="arguments"></a>参数  
`[ @publication = ] 'publication'` 发布的名称。 *发布* 为 **sysname**，默认值为 **%** ，它将清除所有发布的元数据。 如果显式指定发布，则该发布必须已存在。  
  
`[ @reinitialize_subscriber = ] 'subscriber'` 指定是否重新初始化订阅服务器。 *订阅服务器* 为 **nvarchar (5)**，可以为 **true** 或 **FALSE**，默认值 **为 true**。 如果为 **TRUE**，则将订阅标记为要重新初始化。 如果为 **FALSE**，则不将订阅标记为要重新初始化。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 **sp_mergecleanupmetadata** 仅应在包括运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] Service Pack 1 之前版本的服务器的复制拓扑中使用。 仅包含 [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] Service Pack 1 或更高版本的拓扑应使用基于元数据清理的自动保持功能。 运行此存储过程时，请注意运行此存储过程的计算机上的日志文件必然出现和可能出现的大小增长。  
  
> [!CAUTION]
>  执行 **sp_mergecleanupmetadata** 之后，默认情况下，在发布服务器的发布服务器上，所有订阅都存储在 **MSmerge_genhistory** 中， **MSmerge_contents** 和 **MSmerge_tombstone** 标记为要重新初始化，订阅服务器上的所有挂起的更改都将丢失，并且当前快照将被标记为已过时。  
> 
> [!NOTE]
>  如果数据库中有多个发布，并且其中任何一个发布使用无限发布保持期 (**\@ 保留** 期 = **0**) ，则运行 **sp_mergecleanupmetadata** 不会清理数据库的合并复制更改跟踪元数据。 因此，要谨慎使用无限发布保持。  
  
 执行此存储过程时，可以通过将 **\@ reinitialize_subscriber** 参数设置为 **TRUE** (默认) 或 **FALSE** 来选择是否重新初始化订阅服务器。 如果在 **\@ reinitialize_subscriber** 参数设置为 **TRUE** 的情况下执行 **sp_mergecleanupmetadata** ，则即使在创建订阅时没有初始快照 (，也会在订阅服务器上重新应用快照（例如，如果已在订阅服务器) 上手动应用或已存在快照数据和架构）。 应谨慎使用将参数设置为 **FALSE** ，因为如果不重新初始化发布，则必须确保发布服务器和订阅服务器上的数据已同步。  
  
 不管 **\@ reinitialize_subscriber** 的值是什么，如果在调用存储过程时正在进行的合并进程正在尝试将更改上载到发布服务器或重新发布订阅服务器，则 **sp_mergecleanupmetadata** 会失败。  
  
 **执行 reinitialize_subscriber 为 \@ TRUE sp_mergecleanupmetadata：**  
  
1.  建议（不是必需）停止对发布和订阅数据库的所有更新。 如果更新继续进行，则重新初始化发布时，上次合并后在订阅服务器上所做的所有更新都将丢失，但会保留数据收敛。  
  
2.  运行合并代理以执行合并。 建议你在运行合并代理时在每个订阅服务器上使用 **-Validate** 代理命令行选项。 如果运行的是连续模式合并，请参阅本节后面部分 *的连续模式合并的特别注意事项* 。  
  
3.  完成所有合并后，执行 **sp_mergecleanupmetadata**。  
  
4.  使用命名或匿名请求订阅在所有订阅服务器上执行 **sp_reinitmergepullsubscription** ，以确保数据收敛。  
  
5.  如果运行的是连续模式合并，请参阅本节后面部分 *的连续模式合并的特别注意事项* 。  
  
6.  为所有级别涉及的所有合并发布重新生成快照文件。 如果事先没有重新生成快照就试图合并，则会得到提示，要求您重新生成快照。  
  
7.  备份发布数据库。 如果没有这样做，会导致发布数据库还原后合并失败。  
  
 **执行 reinitialize_subscriber 为 \@ FALSE sp_mergecleanupmetadata：**  
  
1.  停止对发布和订阅数据库的 **所有** 更新。  
  
2.  运行合并代理以执行合并。 建议你在运行合并代理时在每个订阅服务器上使用 **-Validate** 代理命令行选项。 如果运行的是连续模式合并，请参阅本节后面部分 *的连续模式合并的特别注意事项* 。  
  
3.  完成所有合并后，执行 **sp_mergecleanupmetadata**。  
  
4.  如果运行的是连续模式合并，请参阅本节后面部分 *的连续模式合并的特别注意事项* 。  
  
5.  为所有级别涉及的所有合并发布重新生成快照文件。 如果事先没有重新生成快照就试图合并，则会得到提示，要求您重新生成快照。  
  
6.  备份发布数据库。 如果没有这样做，会导致发布数据库还原后合并失败。  

 **连续模式合并的特别注意事项**  
  
 如果要运行连续模式合并，则必须执行以下两项操作之一：  
  
-   停止合并代理，然后在不指定 **-连续** 参数的情况下执行另一个合并。  
  
-   通过 **sp_changemergepublication** 停用发布，以确保轮询发布状态的任何连续模式合并都失败。  
  
    ```  
    EXEC central..sp_changemergepublication @publication = 'dynpart_pubn', @property = 'status', @value = 'inactive'  
    ```  
  
 完成运行 **sp_mergecleanupmetadata** 的步骤3后，根据停止的方式恢复连续模式合并。 可以是：  
  
-   为合并代理添加 **-连续流** 参数。  
  
-   用 sp_changemergepublication 重新激活发布 **。**  
  
    ```  
    EXEC central..sp_changemergepublication @publication = 'dynpart_pubn', @property = 'status', @value = 'active'  
    ```  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员或 **db_owner** 固定数据库角色的成员才能执行 **sp_mergecleanupmetadata**。  
  
 若要使用此存储过程，发布服务器运行的必须是 [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)]。 订阅服务器必须运行 [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] 或 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 7.0 Service Pack 2。  
  
## <a name="see-also"></a>另请参阅  
 [MSmerge_genhistory &#40;Transact-sql&#41;](../../relational-databases/system-tables/msmerge-genhistory-transact-sql.md)   
 [MSmerge_contents &#40;Transact-sql&#41;](../../relational-databases/system-tables/msmerge-contents-transact-sql.md)   
 [MSmerge_tombstone &#40;Transact-sql&#41;](../../relational-databases/system-tables/msmerge-tombstone-transact-sql.md)  
  
  
