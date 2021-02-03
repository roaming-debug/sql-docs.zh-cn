---
description: MSSQLSERVER_21879
title: MSSQLSERVER_21879 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 21879 (Database Engine error)
ms.assetid: fcfab735-05ca-423a-89f1-fdee7e2ed8c0
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 68ae3fd91948c5c90e8e17bde3ca57193553977f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196389"
---
# <a name="mssqlserver_21879"></a>MSSQLSERVER_21879
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>详细信息  
  
| Attribute | 值 |  
| :-------- | :---- |  
|产品名称|SQL Server|  
|事件 ID|21879|  
|事件源|MSSQLSERVER|  
|组件|SQLEngine|  
|符号名称|SQLErrorNum21879|  
|消息正文|无法查询重定向服务器“%s”以找到原始发布服务器“%s”和发布服务器数据库“%s”来确定远程服务器的名称；错误 %d，错误消息“%s”。|  
  
## <a name="explanation"></a>说明  
**sp_validate_redirected_publisher** 使用其创建的临时链接服务器连接到重定向发布服务器，以便发现远程服务器的名称。 在链接服务器查询失败时，将返回错误 21879。 对请求远程服务器名称的调用通常是首次使用临时链接服务器，因此如果存在连接问题，则这些问题可能首先会与此调用一起出现。 在远程服务器上，此远程调用只执行选择 **@@servername** 。  
  
用于查询重定向发布服务器的链接服务器使用在为原始发布服务器调用 **sp_adddistpublisher** 时提供的安全模式、登录名和密码。  
  
-   如果使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证（安全模式 0），则使用指定的登录名和密码连接到远程服务器。  
  
-   如果使用 Windows 身份验证（安全模式 1），则对此连接使用可信连接。  
  
    -   如果 **sp_validate_redirected_publisher** 被用户显式调用，则用户运行时所使用的 Windows 登录名将用于该连接。  
  
    -   如果复制代理从 **sp_get_redirected_publisher** 中调用 **sp_validate_redirected_** publisher，则将使用与该代理关联的 Windows 登录名。  
  
错误 21879 可能指示在重定向目标发布服务器上使用未知的登录名调用了 **sp_validate_redirected_publisher**。  
  
## <a name="user-action"></a>用户操作  
请确保 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证登录名或 Windows 身份验证登录名对所有可用性组副本都有效，并具有足够的权限，可访问发布服务器数据库中的订阅元数据表（syssubscriptions 和 sysmergesubscriptions）。  
  
如果由非分发服务器的其他节点上运行的复制代理（如在订阅服务器上运行的合并代理）启动的 **sp_get_redirected_publisher** 调用返回了错误 21879，则应注意一些特殊事项。 如果使用 Windows 身份验证连接到重定向发布服务器，则必须将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 配置为使用 Kerberos 身份验证以便成功建立连接。 当使用 Windows 身份验证且 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 未配置为使用 Kerberos 身份验证时，在订阅服务器上运行的合并代理将收到错误 18456，指示“NT AUTHORITY\ANONYMOUS LOGON”登录名失败。 可以通过三种方式解决此问题：  
  
-   将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 配置为使用 Kerberos 身份验证。 请参阅 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 联机丛书中的 **Kerberos 身份验证和 SQL Server**。  
  
-   使用 **sp_changedistpublisher** 更改与 MSdistpublishers 中的原始发布服务器相关联的安全模式，并指定要用于该连接的登录名和密码。  
  
-   当在分发服务器上调用 **sp_get_redirected_publisher** 时，在合并代理命令行中指定命令行参数 *BypassPublisherValidation* 以跳过验证。  
  
