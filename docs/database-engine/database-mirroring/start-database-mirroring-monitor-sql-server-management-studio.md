---
title: 启动数据库镜像监视器 (SSMS)
description: 介绍如何在 SQL Server Management Studio (SSMS) GUI 中启动数据库镜像监视器。
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- monitoring database mirroring [SQL Server]
- Database Mirroring Monitor [SQL Server], starting
- database mirroring [SQL Server], monitoring
ms.assetid: 53165335-97ca-4f88-8e78-22f1839dee98
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: bb2ffa0d767bd6105a83322f9adf2c3803ba6d50
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352244"
---
# <a name="start-database-mirroring-monitor-sql-server-management-studio"></a>启动数据库镜像监视器 (SQL Server Management Studio)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  数据库镜像监视器是通过 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 启动的 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]监视器的一部分。  
  
> [!NOTE]
>  并非 [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的每个版本都提供数据库镜像监视。 有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]各版本支持的功能列表，请参阅 [SQL Server 2016 各个版本支持的功能](~/sql-server/editions-and-supported-features-for-sql-server-2016.md)。  
  
### <a name="to-launch-the-database-mirroring-monitor"></a>启动数据库镜像监视器  
  
1.  连接到主体服务器实例之后，在对象资源管理器中，单击服务器名称以展开服务器树。  
  
2.  展开 **“数据库”** ，再选择要监视的数据库。  
  
3.  右键单击数据库，选择“任务”  ，再单击“启动数据库镜像监视器”  。  
  
4.  在 **“数据库镜像监视器”** 对话框中，单击 **“注册镜像数据库”** 以注册一个或多个镜像数据库。  
  
    > [!NOTE]  
    >  在一个伙伴上注册数据库时，会自动在另一个伙伴上注册此数据库。 如果监视器已具有另一个伙伴实例的连接凭据，则可以使用这些凭据进行连接。 否则，监视器尝试使用 Windows 身份验证进行连接。 如果要更改用于连接到任一服务器实例的凭据，则单击 **“当单击‘确定’后，显示‘管理服务器连接’对话框”** 。  
  
 有关数据库镜像监视器的详细信息，请参阅 [数据库镜像监视器概述](../../database-engine/database-mirroring/database-mirroring-monitor-overview.md)。  
  
## <a name="see-also"></a>另请参阅  
 [数据库镜像 (SQL Server)](../../database-engine/database-mirroring/database-mirroring-sql-server.md)   
 [使用 Windows 身份验证建立数据库镜像会话 (SQL Server Management Studio)](../../database-engine/database-mirroring/establish-database-mirroring-session-windows-authentication.md)  
  
  
