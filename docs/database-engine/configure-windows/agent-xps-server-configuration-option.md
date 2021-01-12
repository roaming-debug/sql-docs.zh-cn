---
title: “代理 XP”服务器配置选项 | Microsoft Docs
description: 了解如何使用 Agent XP 选项来启用 SQL Server 代理扩展存储过程。 查看使用此选项的示例。
ms.custom: ''
ms.date: 03/02/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- Agent XPs option
- extended stored procedures [SQL Server], SQL Server Agent
ms.assetid: 2e1c6c64-5ce7-4357-98c7-ac7763a9f9de
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 83182354415b7dd442acee73e91ca7e11c20ce74
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98091717"
---
# <a name="agent-xps-server-configuration-option"></a>“代理 XP”服务器配置选项
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  使用 **Agent XPs** 选项可以启用此服务器上的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理扩展存储过程。 如果禁用此选项，则 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 对象资源管理器将不显示 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 代理节点。  
  
 使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 工具启动 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理服务时，会自动启用这些扩展的存储过程。 有关详细信息，请参阅 [Surface Area Configuration](../../relational-databases/security/surface-area-configuration.md)。  
  
> [!NOTE]  
>  [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] 除非这些扩展存储过程在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理服务处于任何状态下都启用，否则对象资源管理器不会显示 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理节点的内容。  
  
 可能的值包括：  
  
-   **0**，表示 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理扩展存储过程不可用（默认值）。  
  
-   **1**，表示 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理扩展存储过程可用。  
  
 该设置立即生效，无需停止并重新启动服务器。  
  
## <a name="example"></a>示例
 下面的示例启用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理扩展存储过程。  

1. 在 Microsoft SQL Server Management Studio 中连接到数据库引擎。

2.  在标准工具栏上，单击“新建查询”。

3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行” 。 
  
```sql 
sp_configure 'show advanced options', 1;  
GO  
RECONFIGURE;  
GO  
sp_configure 'Agent XPs', 1;  
GO  
RECONFIGURE  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [自动执行管理任务（SQL Server 代理）](../../ssms/agent/automated-administration-tasks-sql-server-agent.md)   
 [启动、停止或暂停 SQL Server 代理服务](../../ssms/agent/start-stop-or-pause-the-sql-server-agent-service.md)  
