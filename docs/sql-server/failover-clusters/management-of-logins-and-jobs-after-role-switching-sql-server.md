---
title: 管理镜像故障转移后的登录名和作业
description: 了解如何在将镜像数据库从主数据库故障转移到辅助数据库后，管理登录名和作业。
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: high-availability
ms.topic: how-to
helpviewer_keywords:
- role switching [SQL Server]
ms.assetid: fc2fc949-746f-40c7-b5d4-3fd51ccfbd7b
author: cawrites
ms.author: chadam
ms.openlocfilehash: 1e410b42c5e53b479f28febee1ccd716d68437d8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354011"
---
# <a name="management-of-logins-and-jobs-after-role-switching-sql-server"></a>角色切换后登录名和作业的管理 (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  在为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库部署高可用性或灾难恢复解决方案时，重新生成为 master 或 msdb 数据库中的数据库存储的相关信息十分重要。 通常，相关信息包括主/主体数据库的作业以及连接到数据库所需的用户或进程的登录名。 您应该在承载辅助/镜像数据库的任何 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例上复制此信息。 如果可能，角色切换后最好在新的主/主体数据库中以编程方式重新生成此信息。  
  
## <a name="logins"></a>登录名  
 在承载数据库副本的每个服务器实例上，您都应该重新生成有权访问主体数据库的登录名。 当主/主体角色切换时，只有其登录名在新的主/主体服务器实例上存在的用户才可以访问新的主/主体数据库。 其登录名未在新的主/主体服务器实例上定义的用户是孤立用户，并且无法访问该数据库。  
  
 如果某个用户是孤立用户，请在新的主/主体服务器实例上创建该登录名，并且运行 [sp_change_users_login](../../relational-databases/system-stored-procedures/sp-change-users-login-transact-sql.md)。 有关详细信息，请参阅 [孤立用户故障排除 (SQL Server)](../../sql-server/failover-clusters/troubleshoot-orphaned-users-sql-server.md)。  
  
###  <a name="logins-of-applications-that-use-sql-server-authentication-or-a-local-windows-login"></a><a name="SSauthentication"></a> 使用 SQL Server 身份验证或本地 Windows 登录名的应用程序的登录名  
 如果某一应用程序使用 SQL Server 身份验证或本地 Windows 登录名，则不匹配的 SID 可能会阻止该应用程序的登录名在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的远程实例上解析。 不匹配的 SID 将导致该登录名成为远程服务器实例上的孤立用户。 当应用程序连接到发生故障转移后的镜像数据库或日志传送数据库，或是连接到从备份初始化的复制订阅服务器数据库时，就可能会发生此问题。  
  
 为了避免此问题，我们建议您在设置此类应用程序时采取预防措施，以便使用由 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的远程实例承载的数据库。 预防措施包括将登录名和密码从 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的本地实例传输到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的远程实例。 有关如何避免此问题的详细信息，请参阅知识库文章 918992 —[如何在 SQL Server 实例间传输登录名和密码](https://support.microsoft.com/kb/918992/)。  
  
> [!NOTE]  
>  这个问题会影响不同计算机上的 Windows 本地帐户。 但是，这个问题不会在域帐户上发生，因为 SID 在每台计算机上都是相同的。  
  
 有关详细信息，请参阅 [与数据库镜像和日志传送有关的孤立用户](/archive/blogs/sqlserverfaq/orphaned-users-with-database-mirroring-and-log-shipping) （数据库引擎博客）。  
  
## <a name="jobs"></a>作业  
 作业（如备份作业）需要特殊考虑。 通常，在角色切换后，数据库所有者或系统管理员必须为新的主/主体数据库重新创建作业。  
  
 如果以前的主/主体服务器实例可用，则应该在该 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例上删除原始作业。 请注意，当前镜像数据库上的作业失败，因为它处于 RESTORING 状态，以致不可用。  
  
> [!NOTE]  
>  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的不同实例的配置可能不同，如具有不同的驱动器号等。 每个伙伴的作业必须允许任何这类不同的配置。  
  
## <a name="see-also"></a>另请参阅  
 [当数据库在其他服务器实例上可用时管理元数据 (SQL Server)](../../relational-databases/databases/manage-metadata-when-making-a-database-available-on-another-server.md)   
 [孤立用户故障排除 (SQL Server)](../../sql-server/failover-clusters/troubleshoot-orphaned-users-sql-server.md)  
  
