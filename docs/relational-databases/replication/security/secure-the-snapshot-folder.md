---
description: 保护快照文件夹的安全
title: 保护快照文件夹 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- snapshots [SQL Server replication], security
ms.assetid: 3cd877d1-ffb8-48fd-a72b-98eb948aad27
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 22ec331d7dfa49c521b141a479a29c828a9ac6ac
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97480978"
---
# <a name="secure-the-snapshot-folder"></a>保护快照文件夹的安全
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
  快照文件夹是存储快照文件的目录；建议将该目录专门用于存储快照。 请授予快照代理对该文件夹的写入权限，并确保仅为合并代理或分发代理访问文件夹时所用的 Windows 帐户授予读取权限。 与该代理相关联的 Windows 帐户必须是访问远程计算机上快照文件夹的域帐户。  
  
> [!NOTE]  
>  用户帐户控制 (UAC) 可帮助管理员管理其提升的用户权限（有时称为“特权”） 。 在启用 UAC 的操作系统上运行时，管理员并不使用其管理权限。 相反，他们以标准（非管理）用户的身份执行大多数操作，仅在必要时临时采用其管理权限。 UAC 可以防止对快照共享的管理访问。 因此，必须对快照代理、分发代理和合并代理使用的 Windows 帐户显式授予快照共享权限。 即使 Windows 帐户是管理员组的成员，也必须执行此操作。  
  
 当通过“配置分发向导”或“新建发布向导”来配置分发服务器时，快照文件夹默认为本地路径：X:\Program Files\Microsoft SQL Server\\\<instance>\MSSQL\ReplData。 如果使用远程分发服务器或请求订阅，则必须指定 UNC 网络共享路径（如 \\\\<*computername>* \snapshot），而非本地路径。  
  
 授予对快照文件夹的访问权限时，必须根据对文件夹的访问方式来进行授权。 下面列出了在 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Windows 2003 中使用的对话框选项卡：  
  
-   如果指定本地路径，则可通过该文件夹的 **“属性”** 对话框的 **“安全性”** 选项卡授予权限。  
  
-   如果指定网络共享，则可通过该文件夹的 **“属性”** 对话框的 **“共享”** 选项卡授予权限。  
  
    > [!NOTE]  
    >  如果复制代理在分发服务器上运行，则可使用该文件夹的 **“属性”** 对话框的 **“安全性”** 选项卡对用于运行代理的 Windows 帐户授予权限。 即使使用网络共享也如此。 当发布服务器和分发服务器位于同一台计算机上时，这一点适用于推送订阅的合并代理和分发代理，也适用于快照代理。  
  
 有关为本地路径和网络共享设置权限的详细信息，请参阅 Windows 文档。  
  
> [!NOTE]  
>  如果删除发布，复制将尝试在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 服务帐户的安全上下文中删除快照文件夹。 如果此帐户没有足够的权限，请用具有足够权限的帐户登录，然后手动删除文件夹。 如果文件夹为本地路径，则删除文件夹需要“修改”  权限；如果文件夹为网络路径，则删除文件夹需要“完全控制”  权限。  
  
## <a name="delivering-snapshots-through-ftp"></a>通过 FTP 传递快照  
 建议您采用最安全的方法：将快照存储在 UNC 共享目录中。但也可以将快照存储在 FTP 共享目录中，然后再通过 FTP 传递给订阅服务器。 配置 FTP 服务器时，请确保虚拟目录公开一个基本 UNC 共享目录，该共享目录允许用于发布的快照代理进行写访问。  
  
 若要配置订阅服务器以通过 FTP 检索快照，请先用一个 FTP 登录名和密码设置 FTP 服务器，该 FTP 服务器允许订阅服务器进行读（或“获取”）访问，以便下载快照文件。  
  
 若要通过 FTP 传递快照，请参阅[通过 FTP 传递快照](../../../relational-databases/replication/publish/deliver-a-snapshot-through-ftp.md)。  
  
 有关设置和更改通过 FTP 访问快照的密码的信息，请参阅 [Secure the Publisher](../../../relational-databases/replication/security/secure-the-publisher.md)主题中的“FTP 快照传递”一节。  
  
## <a name="see-also"></a>另请参阅  
 [修改快照选项](../../../relational-databases/replication/snapshot-options.md)   
 [使用快照初始化订阅](../../../relational-databases/replication/initialize-a-subscription-with-a-snapshot.md)   
 [Replication Security Best Practices](../../../relational-databases/replication/security/replication-security-best-practices.md)   
 [查看和修改复制安全设置](../../../relational-databases/replication/security/view-and-modify-replication-security-settings.md)   
 [通过 FTP 传输快照](../../../relational-databases/replication//publish/deliver-a-snapshot-through-ftp.md)  
  
  
