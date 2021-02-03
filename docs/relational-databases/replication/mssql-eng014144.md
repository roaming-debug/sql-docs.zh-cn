---
description: MSSQL_ENG014144
title: MSSQL_ENG014144 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
helpviewer_keywords:
- MSSQL_ENG014144 error
ms.assetid: fdc744d5-530e-48c4-9420-cca032fd482b
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 892f71b9fbfafd6226b32e2f23d8bb85a4d9fe38
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198797"
---
# <a name="mssql_eng014144"></a>MSSQL_ENG014144
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
    
## <a name="message-details"></a>消息详细信息  
  
|Attribute|值|  
|-|-|  
|产品名称|SQL Server|  
|事件 ID|14144|  
|事件源|MSSQLSERVER|  
|组件|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|  
|符号名称||  
|消息正文|无法删除订阅服务器 '%s'。 在发布数据库 '%s' 中已有此服务器的订阅。|  
  
## <a name="explanation"></a>说明  
 配置为订阅服务器的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例在存在有为其配置的活动订阅时无法从订阅服务器的角色中删除。  
  
## <a name="user-action"></a>用户操作  
 在尝试更改 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的订阅服务器状态之前，删除所有相关联的订阅。  
  
1.  在发布服务器的发布数据库中执行 [sp_helpsubscription (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-helpsubscription-transact-sql.md)，以查找订阅。  
  
2.  在发布数据库中执行 [sp_dropsubscription (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-dropsubscription-transact-sql.md)，以删除订阅。  

## <a name="see-also"></a>另请参阅  
 [错误和事件参考（复制）](../../relational-databases/replication/errors-and-events-reference-replication.md)   
 [订阅发布](../../relational-databases/replication/subscribe-to-publications.md)  
  
  
