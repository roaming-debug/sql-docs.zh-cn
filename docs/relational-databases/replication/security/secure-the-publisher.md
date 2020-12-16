---
description: 保护发布服务器的安全
title: 保护发布服务器 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- logins [SQL Server replication], publication access list
- publications [SQL Server replication], publication access lists
- publication access list (PAL)
- PAL (publication access list)
- Publishers [SQL Server replication], security
- publications [SQL Server replication], security
ms.assetid: 4513a18d-dd6e-407a-b009-49dc9432ec7e
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 56788c6ab70a6bb2255e3c6593a774de3f672efa
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97479748"
---
# <a name="secure-the-publisher"></a>保护发布服务器的安全
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
  
以下复制代理将连接到发布服务器：  
  
-   日志读取器代理
-   快照代理
-   队列读取器代理  
-   合并代理  
  
 建议您为这些代理提供合适的登录名，遵循授予所需的最小权限的原则，并保护所有密码的存储。 有关每个代理所需权限的信息，请参阅 [Replication Agent Security Model](../../../relational-databases/replication/security/replication-agent-security-model.md)。  
  
 除适当地管理登录名和密码外，还应了解发布访问列表 (PAL) 的角色。 PAL 用于启用登录名以访问发布数据，同时限制对发布服务器中的数据库进行即席访问。  
  
## <a name="publication-access-list"></a>发布访问列表  
 PAL 是用于保护发布服务器中发布的主要机制。 PAL 的功能与 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Windows 的访问控制列表相似。 创建发布时，复制将为该发布创建 PAL。 可把 PAL 配置为包含被授权访问发布的登录名和组的列表。 当一个代理连接到发布服务器或分发服务器并请求访问某个发布时，PAL 中的身份验证信息将与此代理提供的发布服务器登录名进行比较。 此过程为发布服务器提供了额外的安全性，方法是：阻止客户端工具使用发布服务器和分发服务器登录名在发布服务器中直接进行修改  
  
> [!NOTE]  
>  复制操作在发布服务器中为每个发布创建一个角色，强制应用 PAL 成员身份。 对于合并复制，该角色的名称采用 Msmerge_\<PublicationID> 的形式；对于事务性复制和快照复制，该角色的名称采用 MSReplPAL_\<PublicationDatabaseID>_\<PublicationID> 的形式。  
  
 默认情况下，PAL 中包括的登录名是： **sysadmin** 固定服务器角色在创建发布时的成员，以及用于创建发布的登录名。 默认情况下，发布数据库中属于 **sysadmin** 固定服务器角色或 **db_owner** 固定数据库角色的成员的所有登录名都可订阅发布，而不用显式添加到 PAL 中。  
  
 使用 PAL 时，请考虑以下原则：  
  
-   在将 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 登录名添加到 PAL 前，必须将该登录名与发布数据库中的数据库用户关联。  
  
-   遵循最小权限原则，仅允许 PAL 中的登录名拥有执行复制任务所必需的权限。 不要将登录名添加给执行复制任务所不需要的任何固定数据库角色或服务器角色。 有关所需权限的详细信息，请参阅 [Replication Agent Security Model](../../../relational-databases/replication/security/replication-agent-security-model.md) 和 [Replication Security Best Practices](../../../relational-databases/replication/security/replication-security-best-practices.md)。  
  
-   如果使用远程分发服务器，则 PAL 中的帐户必须在发布服务器和分发服务器中都可用。 帐户必须是在这两个服务器中定义的域帐户或本地帐户。 与这两个登录名关联的密码必须相同。  
  
-   如果 PAL 包含 Windows 帐户并且域使用 Active Directory，则运行 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的帐户必须具有对 Active Directory 的读取权限。 如果遇到与 Windows 帐户有关的问题，请确保运行 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的帐户具有足够的权限。 有关详细信息，请参阅 Windows 文档。  
  
 若要管理 PAL，请参阅[管理发布访问列表中的登录名](../../../relational-databases/replication/security/manage-logins-in-the-publication-access-list.md)。  
  
## <a name="snapshot-agent"></a>快照代理  
 每个发布拥有一个快照代理。 有关详细信息，请参阅 [Create a Publication](../../../relational-databases/replication/publish/create-a-publication.md)。  
  
## <a name="ftp-snapshot-delivery"></a>FTP 快照传递  
 如果指定应通过 FTP 共享而非 UNC 共享使快照可用，则配置 FTP 访问时必须指定登录名和密码。 有关详细信息，请参阅[通过 FTP 传递快照](../../../relational-databases/replication/publish/deliver-a-snapshot-through-ftp.md)。  
  
## <a name="log-reader-agent"></a>日志读取器代理  
 对于使用事务复制发布的每个数据库，都有一个日志读取器代理。 有关详细信息，请参阅 [Create a Publication](../../../relational-databases/replication/publish/create-a-publication.md)。  
  
## <a name="queue-reader-agent"></a>队列读取器代理  
 与给定的分发服务器关联的所有发布服务器和发布（允许排队更新订阅）都拥有一个队列读取器代理。 有关详细信息，请参阅[允许更新事务发布的订阅](../../../relational-databases/replication/publish/enable-updating-subscriptions-for-transactional-publications.md)。  
  
## <a name="see-also"></a>另请参阅  
 [启用数据库引擎的加密连接（SQL Server 配置管理器）](../../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md)   
 [Replication Security Best Practices](../../../relational-databases/replication/security/replication-security-best-practices.md)   
 [查看和修改复制安全设置](../../../relational-databases/replication/security/view-and-modify-replication-security-settings.md)  
  
  
