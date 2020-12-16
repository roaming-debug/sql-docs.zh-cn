---
description: MSSQL_ENG021798
title: MSSQL_ENG021798 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- MSSQL_ENG021798 error
ms.assetid: 596f5092-75ab-4a19-8582-588687c7b089
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: dc233cde39917a65a946b1c4e112fdc0833c48cd
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97469058"
---
# <a name="mssql_eng021798"></a>MSSQL_ENG021798
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
    
## <a name="message-details"></a>消息详细信息  
  
|Attribute|值|  
|-|-|  
|产品名称|SQL Server|  
|事件 ID|21798|  
|事件源|MSSQLSERVER|  
|组件|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|  
|符号名称||  
|消息正文|在继续操作之前，必须通过 '%s' 添加 '%s' 代理作业。 请参阅 '%s' 的文档。|  
  
## <a name="explanation"></a>说明  
 要创建发布，您必须是发布服务器上的 **sysadmin** 固定服务器角色的成员或者发布数据库中的 **db_owner** 固定数据库角色的成员。 如果您是 **db_owner** 角色的成员，则发生以下情况将会引发此错误：  
  
-   从 [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)]运行脚本。 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]中的安全模式已更改，因此必须更新这些脚本。  
  
-   存储过程 **sp_addpublication** 在执行 [sp_addlogreader_agent (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-addlogreader-agent-transact-sql.md) 之前执行。 这适用于所有的事务发布。  
  
-   存储过程 **sp_addpublication** 在执行 [sp_addqreader_agent (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-addqreader-agent-transact-sql.md) 之前执行。 这适用于为排队更新订阅启用的事务发布（sp_addpublication 的 `@allow_queued_tran` 参数的值为 TRUE）。  
  
 存储过程 **sp_addlogreader_agent** 和 **sp_addqreader_agent** 各创建一个代理作业并允许您指定运行代理的 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows 帐户。 对于 **sysadmin** 角色中的用户，如果未执行 **sp_addlogreader_agent** 和 **sp_addqreader_agent** ，则将隐式创建代理作业；代理在分发服务器上的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理服务帐户的上下文中运行。 尽管 **sp_addlogreader_agent** 和 **sp_addqreader_agent** 对于 **sysadmin** 角色中的用户不是必须的，但是为了安全起见，最好为代理指定单独的帐户。 有关详细信息，请参阅 [复制代理安全模式](../../relational-databases/replication/security/replication-agent-security-model.md)。  
  
## <a name="user-action"></a>用户操作  
 请确保按正确的顺序执行过程。 有关详细信息，请参阅 [Create a Publication](../../relational-databases/replication/publish/create-a-publication.md)。 如果有早期版本 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]中的复制脚本，请更新这些脚本以使其包含 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 及更高版本所需的存储过程和参数。 有关详细信息，请参阅[升级复制脚本（复制 Transact-SQL 编程）](../../relational-databases/replication/administration/upgrade-replication-scripts-replication-transact-sql-programming.md)。  
  
## <a name="see-also"></a>另请参阅  
 [错误和事件参考（复制）](../../relational-databases/replication/errors-and-events-reference-replication.md)  
  
  
