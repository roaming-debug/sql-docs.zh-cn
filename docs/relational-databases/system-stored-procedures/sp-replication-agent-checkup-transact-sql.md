---
description: sp_replication_agent_checkup (Transact-SQL)
title: sp_replication_agent_checkup (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_replication_agent_checkup_TSQL
- sp_replication_agent_checkup
helpviewer_keywords:
- sp_replication_agent_checkup
ms.assetid: 50357c2e-71aa-4e13-9e2e-0977a3655cc9
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 77dd179aa4024f969d8e27a3d57cdc03374a4497
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204393"
---
# <a name="sp_replication_agent_checkup-transact-sql"></a>sp_replication_agent_checkup (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  检查每个分发数据库的复制代理，这些代理正在运行但在指定检测信号间隔内没有历史记录。 此存储过程在分发服务器上的任何数据库中执行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_replication_agent_checkup [ [ @heartbeat_interval = ] heartbeat_interval ]  
```  
  
## <a name="arguments"></a>参数  
`[ @heartbeat_interval = ] 'heartbeat_interval'` 代理在不记录进度消息的情况下可以执行的最大分钟数。 *heartbeat_interval* 的值为 **int**，默认值为10分钟。  
  
## <a name="return-code-values"></a>返回代码值  
 对于它检测为可疑的每个代理， **sp_replication_agent_checkup** 引发错误14151。 它还记录关于这些代理的失败历史记录消息。  
  
## <a name="remarks"></a>备注  
 **sp_replication_agent_checkup** 用于快照复制、事务复制和合并复制。  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员才能 **sp_replication_agent_checkup** 执行。  
  
## <a name="see-also"></a>另请参阅  
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
