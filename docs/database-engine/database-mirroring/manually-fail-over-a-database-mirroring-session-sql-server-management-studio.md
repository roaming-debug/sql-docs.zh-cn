---
title: 手动故障转移数据库镜像会话 (SQL Server Management Studio) | Microsoft Docs
description: 了解如何使用 SQL Server Management Studio 启动到镜像服务器的手动故障转移。 然后，镜像数据库变成主体数据库。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- failover [SQL Server], database mirroring
- manual failover [SQL Server]
- database mirroring [SQL Server], failover
ms.assetid: 4ecf9c63-b3a4-4c54-b553-5bc37973232b
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 6f50c8c9de9309069eb45f24bc62532030e18199
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342280"
---
# <a name="manually-fail-over-a-database-mirroring-session-sql-server-management-studio"></a>手动故障转移数据库镜像会话 (SQL Server Management Studio)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  同步镜像数据库时（即数据库处于 SYNCHRONIZED 状态时），数据库所有者可以启动到镜像服务器的手动故障转移。  
  
 在手动故障转移过程中，将交换发生故障转移的数据库的主体服务器和镜像服务器的角色。 镜像数据库变成主体数据库，而主体数据库变成镜像数据库。 例如，下表显示了手动故障转移如何交换下面两个镜像伙伴的角色： `SQLDBENGINE0_1` 和 `SQLDBENGINE0_2`。  
  
|服务器|在故障转移之前|在故障转移之后|  
|------------|---------------------|--------------------|  
|`SQLDBENGINE0_1`|PRINCIPAL|MIRROR|  
|`SQLDBENGINE0_2`|MIRROR|PRINCIPAL|  
  
 注意，其他数据库镜像会话的服务器角色不会受到影响。 有关详细信息，请参阅 [数据库镜像会话期间的角色切换 (SQL Server)](../../database-engine/database-mirroring/role-switching-during-a-database-mirroring-session-sql-server.md)的各版本中均未提供见证服务器实例。  
  
### <a name="to-manually-fail-over-database-mirroring"></a>对数据库镜像进行手动故障转移  
  
1.  连接至主体服务器实例，在 **对象资源管理器** 窗格中，单击服务器名称以展开服务器树。  
  
2.  展开 **“数据库”** ，再选择要进行故障转移的数据库。  
  
3.  右键单击数据库，选择 **“任务”** ，再单击 **“镜像”** 。 这样便可打开 **“数据库属性”** 对话框的 **“镜像”** 页。  
  
4.  单击 **“故障转移”** 。  
  
     将显示一个确认框。  主体服务器将开始尝试使用 Windows 身份验证连接到镜像服务器。 如果 Windows 身份验证无效，主体服务器将显示 **“连接到服务器”** 对话框。 如果镜像服务器使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证，请选择 **“身份验证”** 框中的 **“SQL Server 身份验证”** 。 在 **“登录名”** 文本框中，指定连接镜像服务器时使用的登录帐户，然后在 **“密码”** 文本框中指定该帐户的密码。  
  
     如果故障转移成功， **“数据库属性”** 对话框关闭。 镜像数据库变成主体数据库，而主体数据库变成镜像数据库。  
  
     如果故障转移失败，将显示一条错误消息，并且该对话框保持打开状态。  
  
    > [!IMPORTANT]  
    >  如果打开“镜像”页后修改了某些属性，将不保存这些更改。  
  
     对话框将自动关闭。  
  
## <a name="see-also"></a>另请参阅  
 [数据库属性（“镜像”页）](../../relational-databases/databases/database-properties-mirroring-page.md)   
 [数据库镜像 (SQL Server)](../../database-engine/database-mirroring/database-mirroring-sql-server.md)   
 [手动故障转移数据库镜像会话 (Transact-SQL)](../../database-engine/database-mirroring/manually-fail-over-a-database-mirroring-session-transact-sql.md)   
 [暂停或恢复数据库镜像会话 (SQL Server)](../../database-engine/database-mirroring/pause-or-resume-a-database-mirroring-session-sql-server.md)   
 [删除数据库镜像 (SQL Server)](../../database-engine/database-mirroring/remove-database-mirroring-sql-server.md)  
  
  
