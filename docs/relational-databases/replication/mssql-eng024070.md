---
description: MSSQL_ENG024070
title: MSSQL_ENG024070 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
helpviewer_keywords:
- MSSQL_ENG024070 error
ms.assetid: 23ac7e00-fab6-429b-9f85-2736a322aa65
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: d4d6de8098a2587b54735cd8fcf22c7a4702a7ce
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99180266"
---
# <a name="mssql_eng024070"></a>MSSQL_ENG024070
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
    
## <a name="message-details"></a>消息详细信息  
  
|Attribute|值|  
|-|-|  
|产品名称|SQL Server|  
|事件 ID|24070|  
|事件源|MSSQLSERVER|  
|组件|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|  
|符号名称||  
|消息正文|客户端没有所需的特权。|  
  
## <a name="explanation"></a>说明  
 这是一个常规错误，不管是否进行复制，都会引发该错误。 对于复制拓扑中的服务器，引发该错误的原因通常是使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Windows 服务控制管理器，而不是 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 配置管理器来更改 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理服务帐户。 当您在更改服务帐户后尝试运行代理作业时，作业可能会失败，并显示类似如下的错误消息：  
  
 `Executed as user: \<UserAccount>. Replication-Replication Snapshot Subsystem: agent \<AgentName> failed. Executed as user: \<UserAccount>. A required privilege is not held by the client. The step failed. [SQLSTATE 42000] (Error 14151). The step failed.`  
  
 出现此问题的原因是 Windows 服务控制管理器无法向 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理的新服务帐户授予所需权限。  
  
## <a name="user-action"></a>用户操作  
 为了避免以后再出现此问题，请始终使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 配置管理器而非 Windows 服务控制管理器来更改服务帐户和密码。  
  
 若要解决此问题，请使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 配置管理器将服务帐户更改为原始帐户。 然后，使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 配置管理器更改为新帐户。 执行此操作时， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 配置管理器会将新帐户添加到以下安全组中：  
  
 SQLServer2008SQLAgentUser$ComputerName$InstanceName  
  
 成为此安全组的成员，便可以向新帐户授予运行复制代理作业所需的权限。  
  
## <a name="see-also"></a>另请参阅  
 [错误和事件参考（复制）](../../relational-databases/replication/errors-and-events-reference-replication.md)   
 [管理复制中的登录名和密码](../../relational-databases/replication/security/identity-and-access-control-replication.md)   
 [SQL Server 配置管理器](../../relational-databases/sql-server-configuration-manager.md)  
  
  
