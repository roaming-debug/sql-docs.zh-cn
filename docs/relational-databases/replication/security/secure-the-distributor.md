---
description: 保护分发服务器的安全
title: 保护分发服务器 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- security [SQL Server replication], Distributors
- Distributors [SQL Server replication], security
ms.assetid: 76d78229-0ff2-4aa4-9b4e-ad97534c5296
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: f8a6ff28260945bc3468d11514eb63e404a38583
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97475688"
---
# <a name="secure-the-distributor"></a>保护分发服务器的安全
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
  下列复制代理连接到分发服务器：日志读取器代理、快照代理、队列读取器代理、分发代理以及合并代理。 在遵守授予必要的最低权限并保护所有密码的存储这一原则的同时，为每个代理提供适当的登录名非常重要。  
  
-   有关管理登录名和密码的信息，请参阅[管理复制中的登录名和密码](../../../relational-databases/replication/security/identity-and-access-control-replication.md)。  
  
-   有关每个代理所需权限的详细信息，请参阅 [Replication Agent Security Model](../../../relational-databases/replication/security/replication-agent-security-model.md)。  
  
 除了适当地管理登录名和密码之外，了解 **repl_distributor** 远程服务器链接和 **distributor_admin** 帐户的角色也很重要。  
  
## <a name="securing-the-connection-from-the-publisher-to-the-distributor"></a>保护发布服务器到分发服务器的连接  
 为了支持管理存储过程在发布服务器上执行并更新分发服务器中的信息时所要的通信，复制将自动配置远程服务器 **repl_distributor**。 **repl_distributor** 远程服务器项用于与分发数据库的通信，而不管此分发数据库是包含在发布服务器实例（本地分发服务器）内，还是驻留在远程 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例（远程分发服务器）内。  
  
 当分发数据库包含在本地实例上时，密码将随机产生并自动配置。 当分发数据库位于远程实例中时，管理员需要在设置发布服务器和分发服务器期间配置共享密码；然后，此密码将用于为遍历 **repl_distributor** 链接的通信流量提供身份验证。  
  
 分发服务器可以使用 **repl_distributor** 远程服务器项来验证调用服务器是否在分发服务器上配置为发布服务器，可以验证该发布服务器提供的密码并验证存储过程在执行期间是否为复制存储过程。  
  
 设置期间为 **repl_distributor** 远程服务器项配置的密码与 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的登录名 **distributor_admin** 相关联，此登录名将添加到分发服务器的 **sysadmin** 固定服务器角色中。 连接到分发服务器时，复制存储过程将使用 **distributor_admin** 登录名。  
  
> [!NOTE]  
>  不要手动更改 **distributor_admin** 的密码。 始终使用 **中的** sp_changedistributor_password **存储过程、** “分发服务器属性” **或** “更新复制密码” [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)]对话框，因为密码更改随后将自动应用到本地发布。  
  
## <a name="snapshot-folder-security"></a>快照文件夹的安全性  
 确保快照共享将读权限授予合并代理（对于合并发布）或分发代理（对于快照复制或事务复制）运行时所用的帐户，并将写权限授予快照代理运行时所用的帐户。 有关快照文件夹的详细信息，请参阅[保护快照文件夹](../../../relational-databases/replication/security/secure-the-snapshot-folder.md)。  
  
## <a name="see-also"></a>另请参阅  
 [查看和修改复制安全设置](../../../relational-databases/replication/security/view-and-modify-replication-security-settings.md)   
 [启用数据库引擎的加密连接（SQL Server 配置管理器）](../../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md)   
 [Replication Security Best Practices](../../../relational-databases/replication/security/replication-security-best-practices.md)   
 [查看和修改复制安全设置](../../../relational-databases/replication/security/view-and-modify-replication-security-settings.md)  
  
  
